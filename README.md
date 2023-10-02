# Ansible Playbook: Windows NT 4 on Uberspace

> Heavily inspired by https://github.com/aaronk6/win311-uberspace-ansible.

This playbook will set up an Uberspace to host a Windows NT 4 VM running a web server on port 80 and make it available behind a per-user nginx service. QEMU will be used for virtualisation.

**You will need to provide your own HDD image for the VM.** It’s not included here, but the playbook will upload it for you if you specify the path.

## Why?

Why not?

### Why Windows NT 4?

It’s the first version of Windows that included a fully-featured IIS with HTTP, FTP and Gopher support as well as integration with FrontPage for authoring.

## Run Playbook

```
ansible-playbook site.yml
```

This will prompt for the path to `c.img` (the HDD image for the VM). Alternatively, you can specify the path on the command line to avoid the prompt:

```
ansible-playbook site.yml -e 'local_drive_c_img="/Users/rubenarakelyan/Documents/Images/c.img"'
```

**Note:** The single quotes are only required if your path contains spaces.

## VNC to Windows VM

Establish an SSH tunnel to Uberspace (assuming your Uberspace is winnt4.example.com):

```
ssh -L 5901:localhost:5901 winnt4.example.com
```

Then use a local VNC client to connect to `localhost:5901`. On macOS, this works with RealVNC (VNC Viewer.app), but not with the integrated screen sharing app.

## Telnet to QEMU Monitor

Use this for committing changes to `c.img`. By default, changes are discarded on exit (snapshot mode).

```
ssh -L 4444:localhost:4444 winnt4.example.com
```

Then locally:

```
telnet localhost 4444
commit ide0-hd0
```

Download image:

```
scp winnt4.example.com:winnt4/c.img .
```
