# FTP Server

The **FTP server** is used by CTA for transferring files to/from University Libraries using FTP or secure file transfer. The service is deployed on a dedicated EC2 instance.

Supported use cases include:

* Transfer of ETD submissions from ProQuest (SSH file transfer)
* Transfer of bursar out files from Sierra (FTP)

As ProQuest uses secure file transfer, its public key is contained in the authorized_keys file. It's corresponding user account (proquest) does not use a password (this is the preferred method to connect to an EC2 instance).

Sierra relies on FTP to transfer files and therefore has a password-enabled user account (sierra). There is no expiry date on the password. Credentials are stored in Keepass.

## Infrastructure

The FTP service is hosted on a single AWS EC2 instance (ftp-prod) contained within the US West 2 production VPC. Its public-facing IP address is 54.187.105.7. Refer to the AWS console for other instance details.

External traffic to/from the instance is controlled by the security group cubl-ftp-sg.

File storage is provided by an EFS network file service (FTPFS). Each use case is provided its own directory for file storage, e.g., /data/proquest and /data/sierra. Further delineation between production and test is provided by corresponding access points. For example, in a production environment, the access point /prod is defined and the local mount point is /data. The access point for testing purposes is /test. Access to/from EFS is controlled by security group cubl-ftpfs-sg.  

## FTP service

While SSH file transfer is the preferred method to send files from an external service provider, there may be situations where FTP is the only option. In that case, an FTP daemon is installed on this server to handle file transfers. This service is **vsftpd** or Very Secure FTP Daemon. The behavior of the service is controlled by a single configuration file located at `/etc/vsftpd/vsftpd.conf`. Refer to <https://github.com/culibraries/ftp-server> for the current configuration settings.

The preferred FTP method is explicit FTP over TLS, which is more secure than plain FTP. This method ensures that traffic between the client and server is encrypted and secure. The support of encrypted connections requires an SSL certificate. This can be generated using openssl or one can be generated/purchased elsewhere. For this installation, the SSL key is placed at `/etc/ssl/private/vsftpd.key` and the certificate is located at `/etc/ssl/certs/vsftpd.crt`. The SSL settings in vsftpd.conf handle the rest. Refer to [Configure VSFTPD with an SSL](https://liquidweb.com/kb/configure-vsftpd-ssl) for details on how to set this up.

NOTE: The certificate was recently renewed (10/25/2021) for a two-year period.

Refer to the [manpage](https://linux.die.net/man/8/vsftpd) for the various configuration options and allowable values. Another good resource (with examples) is available at the [Ubuntu Community Help Wiki](https://help.ubuntu.com/community/vsftpd).
