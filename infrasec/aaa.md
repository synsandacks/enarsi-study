## Authentication Authorization and Accounting (AAA)

AAA services provide a means for enhanced security on network infrastructure. By implementing AAA you gain additional control and visibility into what users are doing when accessing network infrastructure. 

- Authentication - Who are you?
- Authorization - What can you do? 
- Accounting - Logging of what you did

AAA can be implemented using the local database or a AAA server. For AAA services to a remote server you have two protocols RADIUS and TACACS. 

* RADIUS
    * Open standard
    * Default UDP Ports - 1812 (Authentication) and 1813 (Accounting)
    * Authentication and Authorization are combined. - No additional checks on what you're permitted to do once you've authenticated. 
    * Typically leveraged for controlling network access. However, it can be used for controlling device administration. 


* TACACS
    * Cisco proprietary
    * TCP port 49
    * Authentication and Authorization are separated - Just because you're authenticated doesn't mean you are authorized to execute commands.
    * Used for controlling device Administration

## Basic Configuration

Prior to implementing AAA it's important to have a local account defined and the enable password set. 

```
enable secret enp@ssw0rd!
username admin privilege 15 secret p@ssw0rd!
```

### Enable AAA

```
aaa new-model
```

### Configure AAA Server

In the example I will configure a TACACS server. Configuration is very similar for a RADIUS server. 

```
tacacs server LABTACSERVER
 address ipv4 10.10.10.20
 key t@ckey1!
```

### Add Server to Group

Creating server groups allow you to reference a group of TACACS servers in an authentication method list or authorization list.

```
aaa group server tacacs+ LABTACSERVERS
 server name LABTACSERVER
```

### Create an Authentication Method List

In the example below I will configure the default authentication method list with a fallback option for local authentication. It's important to have a fallback method defined in scenarios where your authentication servers might become unreachable. (Yes this happens even in the most resilient of networks.)

```
aaa authentication login default group LABTACSERVERS local
```

### Create an Authorization Method List

When creating authorization method lists for device management functions it is common to configure authorization for *exec* and for *commands*. In the example below I will configure authorization to start the exec shell. Additionally I will add a fall-back of local. This allows a local account or user to have authorization to start a shell after a successful authentication even if the AAA server is unreachable. If the local fallback option or *if-authenticated* options aren't added to the authorization method list the local account will not be able to get an exec shell even upon successful authentication. 

```
aaa authorization exec default group LABTACSERVERS local
```

### Create an Accounting Method List

If you want to log what a user does while authenticated / authorized to your network device it's important to configure accounting. When configuring accounting of commands on a device you do so on a per privilege level basis.

```
aaa accounting commands 15 default start-stop group LABTACSERVERS
```

### Applying AAA methods For Device Access

After creating the Authentication and Authorization method lists they need to be applied to take effect. If you are applying AAA authorization to the console it's important to configure the global configuration command of *aaa authorization console* otherwise the line console 0 sub configuration mode command will not take effect.

```
LABROUTER(config-line)#authorization exec default
%Authorization without the global command 'aaa authorization console' is useless
```

Example applying the default AAA methods to the VTY lines:

```
line vty 0 4
login authentication default
authorization exec default
accounting commands 15 default
```
