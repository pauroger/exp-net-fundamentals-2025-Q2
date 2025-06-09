# Journal

## Blocking Outbound Traffic with Linux Firewalls

Today I learned how to block outbound traffic on Linux using firewall tools:
`ufw` on Ubuntu and `firewalld` on Red Hat. As a test, we attempted to block
access to aardwolf.org on TCP port 4000 — a public MUD server.

- `ufw` makes it easy to block outbound traffic by port on Ubuntu.
- `firewalld` requires more detailed rules and is sensitive to system-level
  support.

```sh
root@ip-10-200-123-5:/home/ubuntu# telnet aardwolf.org 4000
Trying 23.111.142.226...
Connected to aardwolf.org.
Escape character is '^]'.
#############################################################################
##[                                               ]##########################
##[        --- Welcome to Aardwolf MUD ---        ]############ /"  #########
##[                                               ]########  _-`"""', #######
##[         Players Currently Online: 195         ]#####  _-"       )  ######
##[                                               ]### _-"          |  ######
################################################### _-"            ;  #######
######################################### __---___-"              |  ########
######################################  _"   ,,                  ;  `,,  ####
#################################### _-"    ;''                 |  ,'  ; ####
##################################  _"      '                    `"'   ; ####
###########################  __---;                                 ,' ######
######################## __""  ___                                ,' ########
#################### _-""   -"" _                               ,' ##########
################### `-_         _                              ; ############
#####################  ""----"""   ;                          ; #############
#######################  /          ;                        ; ##############
#####################  /             ;                      ; ###############
###################  /                `                    ; ################
#################  /                                      ; #################
-----------------------------------------------------------------------------
    Enter your character name or type 'NEW' to create a new character
-----------------------------------------------------------------------------
What be thy name, adventurer? Pau

Connection closed by foreign host.

root@ip-10-200-123-5:/home/ubuntu# ufw deny out 4000/tcp
Rule added
Rule added (v6)
root@ip-10-200-123-5:/home/ubuntu# ufw status
Status: active

To                         Action      From
--                         ------      ----
23/tcp                     DENY OUT    Anywhere
4000/tcp                   DENY OUT    Anywhere
23/tcp (v6)                DENY OUT    Anywhere (v6)
4000/tcp (v6)              DENY OUT    Anywhere (v6)

root@ip-10-200-123-5:/home/ubuntu# telnet aardwolf.org 4000
Trying 23.111.142.226...
```

RedHat Example:

```sh
[root@ip-10-200-123-14 ec2-user]# telnet aardwolf.org 4000
Trying 23.111.142.226...
Connected to aardwolf.org.
Escape character is '^]'.
#############################################################################
##[                                               ]##########################
##[        --- Welcome to Aardwolf MUD ---        ]############ /"  #########
##[                                               ]########  _-`"""', #######
##[         Players Currently Online: 200         ]#####  _-"       )  ######
##[                                               ]### _-"          |  ######
################################################### _-"            ;  #######
######################################### __---___-"              |  ########
######################################  _"   ,,                  ;  `,,  ####
#################################### _-"    ;''                 |  ,'  ; ####
##################################  _"      '                    `"'   ; ####
###########################  __---;                                 ,' ######
######################## __""  ___                                ,' ########
#################### _-""   -"" _                               ,' ##########
################### `-_         _                              ; ############
#####################  ""----"""   ;                          ; #############
#######################  /          ;                        ; ##############
#####################  /             ;                      ; ###############
###################  /                `                    ; ################
#################  /                                      ; #################
-----------------------------------------------------------------------------
    Enter your character name or type 'NEW' to create a new character
-----------------------------------------------------------------------------
What be thy name, adventurer? Pau
Connection closed by foreign host.

[root@ip-10-200-123-14 ec2-user]# sudo firewall-cmd --permanent --direct --add-rule ipv4 filter OUTPUT 0 -p tcp --dport 4000 -j REJECT
success
[root@ip-10-200-123-14 ec2-user]# sudo firewall-cmd --reload
success
[root@ip-10-200-123-14 ec2-user]# telnet aardwolf.org 4000
Trying 23.111.142.226...
^C
```
