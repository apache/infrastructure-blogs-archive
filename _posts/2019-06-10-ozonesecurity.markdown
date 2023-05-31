---
layout: post
title: Securing Hadoop Ozone
date: '2019-06-10T00:00:00+00:00'
categories: ozonesecurity
---
<h1>Security in Apache Hadoop Ozone - 1</h1>
<p>Apache Hadoop Ozone is a highly scalable distributed object store for big data applications [1]. This blog post provides an overview of Ozone security and details required to set up secure Ozone cluster.</p>
<p>The Ozone security architecture is described in detail in [2].</p>
<p><strong>Authentication</strong>, <strong>Authorization and Auditing</strong> are three basic tenets of security. Ozone security design borrows heavily from Apache Hadoop security. Having said that, there are areas where Ozone security differs from Hadoop. Let&rsquo;s have a closer look at what this exactly means.</p>
<p><strong>Authentication</strong></p>
<p>Similar to hadoop, Ozone allows kerberos-based authentication. So one way to setup identities for all the daemons and clients is to create kerberos keytabs and configure it like any other service in hadoop.</p>
<p>Below are some important configurations to configure kerberos security.</p>
<p>hdds.scm.kerberos.principal=scm/scm@EXAMPLE.COM</p>
<p>hdds.scm.kerberos.keytab.file=/etc/security/keytabs/scm.keytab</p>
<p>ozone.om.kerberos.principal=om/om@EXAMPLE.COM</p>
<p>ozone.om.kerberos.keytab.file=/etc/security/keytabs/om.keytab</p>
<p>hdds.scm.http.kerberos.principal=HTTP/scm@EXAMPLE.COM</p>
<p>hdds.scm.http.kerberos.keytab=/etc/security/keytabs/HTTP.keytab</p>
<p>ozone.om.http.kerberos.principal=HTTP/om@EXAMPLE.COM</p>
<p>ozone.om.http.kerberos.keytab=/etc/security/keytabs/HTTP.keytab</p>
<p><strong>Certificates</strong></p>
<p>Apart from kerberos and tokens Ozone utilizes certificate based authentication for Ozone service components. To enable this, SCM (StorageContainerManager) bootstraps itself as an Certificate Authority when security is enabled. This allows all daemons inside Ozone to have an SCM signed certificate. Below is brief descriptions of steps involved:</p>
<ol>
<li>Datanodes and OzoneManagers submits a CSR (certificate signing request) to SCM.</li>
<li>SCM verifies identity of DN (Datanode) or OM via Kerberos and generates a certificate.</li>
<li>This certificate is used by OM and DN to prove their identities.</li>
<li>Datanodes use OzoneManager certificate to validate block tokens. This is possible because both of them trust SCM signed certificates. (i.e OzoneManager and Datanodes)</li>
</ol>
<p><strong>Tokens</strong></p>
<p>Tokens are widely used in distributed systems as mean to achieve lightweight authentication without compromising on security. Main motivation for using tokens inside Ozone is to prevent the unauthorized access while keeping the protocol lightweight and without sharing secret over the wire. Ozone utilizes three types of token:</p>
<p><strong>Delegation token</strong></p>
<p>Once client establishes their identity via kerberos they can request a delegation token from OzoneManager. This token can be used by a client to prove its identity until the token expires. Like Hadoop delegation tokens, an Ozone delegation token has 3 important fields:</p>
<p>Renewer: User responsible for renewing the token.</p>
<p>Issue date: Time at which token was issued.</p>
<p>Max date: Time after which token can&rsquo;t be renewed.</p>
<p>Token operations like get, renew and cancel can only be performed over an Kerberos authenticated connection. Clients can use delegation token to establish connection with OzoneManager and perform any file system/object store related operations like, listing the objects in a bucket or creating a volume etc.</p>
<p><strong>Block Tokens</strong></p>
<p>Block tokens are similar to delegation tokens in sense that they are signed by OzoneManager. But this is where similarity between two stops. Block tokens are created by OM (OzoneManager) when a client request involves interaction with DataNodes. Unlike delegation tokens there is no client API to request block tokens. Instead they are handled transparently for client. Block tokens are embedded directly into client request responses. It means that clients don&rsquo;t need to fetch it explicitly from Ozone Manager. This is handled implicitly inside ozone client. Datanodes validates block tokens from clients for every client connection. Below sequence diagram shows steps involved in block token.</p>
<p><br /><a href="https://blogs.apache.org/ozonesecurity/mediaresource/dde0b443-4674-4c5e-9b90-06864297b1c5"><img src="https://blogs.apache.org/ozonesecurity/mediaresource/dde0b443-4674-4c5e-9b90-06864297b1c5?t=true" alt="image2.png"></img></a><br /></p>
<p><strong>S3Token</strong></p>
<p>Like block tokens S3Tokens are handled transparently for clients. It is signed by S3secret created by client. S3Gateway creates this token for every s3 client request. To create an S3Token user must have a S3 secret. Below sequence diagram shows steps involved in s3 secret and S3 token usage.<br /><br /></p><a href="https://blogs.apache.org/ozonesecurity/mediaresource/9a0a4614-742c-4a25-9b7e-dd04409e775f">
<img src="https://blogs.apache.org/ozonesecurity/mediaresource/9a0a4614-742c-4a25-9b7e-dd04409e775f?t=true" alt="image3.jpg"></img></a>

<p><br /><a href="https://blogs.apache.org/ozonesecurity/mediaresource/1388187b-4f08-4c3b-9bc7-58ad6d872071"><img src="https://blogs.apache.org/ozonesecurity/mediaresource/1388187b-4f08-4c3b-9bc7-58ad6d872071?t=true" alt="image1.png"></img></a><br /></p>
<p><strong>Authorization</strong></p>
<p>Ozone provides a pluggable API to control authorization of all client related operations. Default implementation allows every request. Clearly it is not meant for production environments. To configure a more fine grained policy one may configure Ranger plugin for Ozone. Since it is a pluggable module clients can also implement their own custom authorization policy and configure it using [ozone.acl.authorizer.class].</p>
<p><strong>Audit</strong></p>
<p>Ozone provides ability to audit all read &amp; write operations to OM, SCM and Datanodes. Ozone audit leverages the Marker feature which enables user to selectively audit only READ or WRITE operations by a simple config change without restarting the service(s).</p>
<p>To enable/disable audit of READ operations, set filter.read.onMatch to NEUTRAL or DENY respectively. Similarly, the audit of WRITE operations can be controlled using filter.write.onMatch.</p>
<p>Generating audit logs is only half the job, so Ozone also provides AuditParser - a sqllite based command line utility to parse/query audit logs with predefined templates(ex. Top 5 commands) and options for custom query. Once the log file has been loaded to AuditParser, one can simply run a template as shown below:</p>
<p>ozone auditparser &lt;path to db file&gt; template top5cmds</p>
<p>Similarly, users can also execute custom query using:</p>
<p>ozone auditparser &lt;path to db file&gt; query "select * from audit where level=='FATAL'"</p>
<p><br /><br /></p>
<p><strong>How to enable security in Ozone?</strong></p>
<p>To turn on Ozone security set &ldquo;ozone.security.enabled&rdquo; to true. Below is list of important properties for Ozone security:</p>
<p><br /><br /></p>
<table>
<tbody>
<tr>
<td>
<p>ozone.security.enabled</p>
</td>
<td>
<p>True if security is enabled for Ozone. When this property is true, hadoop.security.authentication should be Kerberos.</p>
</td>
</tr>
<tr>
<td>
<p>hdds.scm.kerberos.principal</p>
</td>
<td>
<p>The SCM service principal. Ex scm/_HOST@REALM.COM</p>
</td>
</tr>
<tr>
<td>
<p>hdds.scm.kerberos.keytab.file</p>
</td>
<td>
<p>The keytab file used by SCM daemon to login as its</p>
<p>service principal.</p>
</td>
</tr>
<tr>
<td>
<p>ozone.om.kerberos.principal</p>
</td>
<td>
<p>The OzoneManager service principal. Ex om/_HOST@REALM.COM</p>
</td>
</tr>
<tr>
<td>
<p>ozone.om.kerberos.keytab.file</p>
</td>
<td>
<p>The keytab file used by SCM daemon to login as its</p>
<p>service principal.</p>
</td>
</tr>
<tr>
<td>
<p>hdds.scm.http.kerberos.principal</p>
</td>
<td>
<p>SCM http server service principal.</p>
</td>
</tr>
<tr>
<td>
<p>hdds.scm.http.kerberos.keytab</p>
</td>
<td>
<p>The keytab file used by SCM http server to login as its</p>
<p>service principal.</p>
</td>
</tr>
<tr>
<td>
<p>ozone.om.http.kerberos.principal</p>
</td>
<td>
<p>OzoneManager http server principal.</p>
</td>
</tr>
<tr>
<td>
<p>ozone.om.http.kerberos.keytab</p>
</td>
<td>
<p>The keytab file used by OM http server to login as its</p>
<p>service principal.</p>
</td>
</tr>
</tbody>
</table>
<p><br /><br /><br /></p>
<p><strong>References:</strong></p>
<ol>
<li>Apache Hadoop Ozone website: <a href="https://hadoop.apache.org/ozone/">https://hadoop.apache.org/ozone/</a></li>
<li><a href="https://issues.apache.org/jira/secure/attachment/12911638/HadoopStorageLayerSecurity.pdf">HDDS security design document</a></li>
<li><a href="https://issues.apache.org/jira/browse/HDDS-4">https://issues.apache.org/jira/browse/HDDS-4</a></li>
</ol>
<p><br /><br /></p>
