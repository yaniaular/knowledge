Configurar servidor Samba
=========================

Install Samba
-------------

    sudo apt-get update
    sudo apt-get install samba

Set a password for your user in Samba
-------------------------------------

    sudo smbpasswd -a <user_name>

        Note: Samba uses a separate set of passwords than the standard Linux system accounts (stored in /etc/samba/smbpasswd), so you'll need to create a Samba password for yourself. This tutorial implies that you will use your own user and it does not cover situations involving other users passwords, groups, etc...

        Tip1: Use the password for your own user to facilitate.

        Tip2: Remember that your user must have permission to write and edit the folder you want to share.
        Eg.:
        sudo chown <user_name> /var/opt/blah/blahblah
        sudo chown :<user_name> /var/opt/blah/blahblah

        Tip3: If you're using another user than your own, it needs to exist in your system beforehand, you can create it without a shell access using the following command :
        sudo useradd USERNAME --shell /bin/false

        You can also hide the user on the login screen by adjusting lightdm's configuration, in /etc/lightdm/users.conf add the newly created user to the line :
        hidden-users=

Create a directory to be shared
-------------------------------

mkdir /home/<user_name>/<folder_name>

Make a safe backup copy of the original smb.conf file to your home folder, in case you make an error
----------------------------------------------------------------------------------------------------

sudo cp /etc/samba/smb.conf ~

Edit the file "/etc/samba/smb.conf"
-----------------------------------

sudo nano /etc/samba/smb.conf

    Once "smb.conf" has loaded, add this to the very end of the file:

    [<folder_name>]
    path = /home/<user_name>/<folder_name>
    available = yes
    valid users = <user_name>
    read only = no
    browseable = yes
    public = yes
    writable = yes

    Tip: There Should be in the spaces between the lines, and note que also there should be a single space both before and after each of the equal signs.

Restart the samba:
------------------

sudo service smbd restart

Once Samba has restarted, use this command to check your smb.conf for any syntax errors
---------------------------------------------------------------------------------------

testparm

To access your network share
----------------------------

To access your network share use your username (<user_name>) and password through the path "smb://<HOST_IP_OR_NAME>/<folder_name>/" (Linux users) or "\\<HOST_IP_OR_NAME>\<folder_name>\" (Windows users). Note that "<folder_name>" value is passed in "[<folder_name>]", in other words, the share name you entered in "/etc/samba/smb.conf". 

Acceder a servidor samba desde cliente
======================================

Samba Client - Manual Configuration

This section covers how to manually configure and connect to a SMB file server from an Ubuntu client. smbclient is a command line tool similar to a ftp connection while smbfs allows you to mount a SMB file share. Once a SMB share is mounted it acts similar to a local hard drive (you can access the SMB share with your file browser (nautilus, konqueror, thunar, other).

Connecting to a Samba File Server from the command line

Connecting from the command line is similar to a ftp connection.

List public SMB shares with

**smbclient -L //server -U user**

Connect to a SMB share with

**smbclient //server/share -U user**

Enter you user password.

You can connect directly with

**smbclient //server/share -U user%password**

but your password will show on the screen (less secure).

Once connected you will get a prompt that looks like this :

**smb: \>**



Ejemplo
=======

Desde el servidor Agregar al final

**sudo vim /etc/samba/smb.conf**

[yanina]
   comment = Disco Yani Main
   path = /home/yanina/
   browseable = yes
   read only = no
   available = yes
   valid users = yanina
   public = yes
   writable = yes

**sudo service smbd restart**
**testparm**

Desde el cliente

**sudo apt-get install smbclient**

Listar servidores disponibles

**smbclient -L 10.0.0.6** Ip de la maquina a la que te quieres conectar

Domain=[WORKGROUP] OS=[Unix] Server=[Samba 4.1.13-Ubuntu]

        Sharename       Type      Comment
        ---------       ----      -------
        yanina          Disk      Disco Yani Main
        print$          Disk      Printer Drivers
        IPC$            IPC       IPC Service (Samba 4.1.13-Ubuntu)
        Hewlett-Packard-HP-LaserJet-P2015-Series Printer   Hewlett-Packard HP LaserJet P2015 Series
        Samsung-C460-Series Printer   Samsung C460 Series
Domain=[WORKGROUP] OS=[Unix] Server=[Samba 4.1.13-Ubuntu]

        Server               Comment
        ---------            -------
        KATHY-VAUXOO         kathy-vauxoo server (Samba, Ubuntu)
        YANINA-LINUX         Samba 4.1.13-Ubuntu

        Workgroup            Master
        ---------            -------
        WORKGROUP            KATHY-VAUXOO


**smbclient //10.0.0.6/yanina -U yanina**
