---
layout: post
title: Write-only sFTP server 
description: Using ProFTPD to setup a sFTP server that allows uploads but not downloads.
---

I wanted an SFTP that could be used for upload only: Users can't even get directory listings from the servers. They can only create directories and upload files. I tried to do this with OpenSSH to no avail, and instead used ProFTPD.  ProFTPD allows you to place restrictions on the commands that users can issue.  The client code uses WinSCP to upload files to the server, and is written in C#:

{% highlight C# %}
using (Session session = new Session())
{
    session.Open(sessionOptions);
    session.CreateDirectory(subdirectory);
    transferResult = session.PutFiles(localFile, remoteFile, false, transferOptions);
    transferResult.Check();
}
{% endhighlight %}

To figure out the minimum necessary set of commands, I enabled the extended log in /etc/proftpd/proftpd.conf:

    ExtendedLog /var/log/proftpd/extended.log ALL

Running the client then showed all the commands the client executed in the log file.  Locking down the server was as simple as only allowing these commands to execute:

{% highlight apache %}
# Disable all commands
<Limit ALL>
DenyAll
</Limit>
# Enable only required commands
<Limit CDUP CWD PWD XCWD XCUP STOR STOU >
AllowAll
</Limit>
# A command I only saw in WinSCP and not with Putty.  Required for WinSCP to work.
<Limit REALPATH >
AllowAll
</Limit>
# Allow directory creation
<Limit MKD >
AllowAll
</Limit>
{% endhighlight %}
 

