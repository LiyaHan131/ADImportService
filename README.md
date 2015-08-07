# Kentico AD Import Service

[![Join the chat at https://gitter.im/Kentico/ADImportService](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/Kentico/ADImportService?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)
[![Build status](https://ci.appveyor.com/api/projects/status/jin5kt2gx4co2gre?svg=true)](https://ci.appveyor.com/project/kentico/adimportservice)

Kentico Active Directory Import Service provides real-time import of users and groups from the Active Directory database to users and roles in a Kentico. The service is fully configurable using a configuration file.


## Installation

Assume that you have Kentico version 8.x installed, follow these steps:

1. Enable REST service in Kentico settings with basic authentication
2. Download the [ADImportService.exe](https://github.com/Kentico/ADImportService/releases/latest) executable from releases
3. Open the command line and find the ```InstallUtil``` utility (most likely in ```C:\Windows\Microsoft.NET\Framework64\v4.0.x```
4. Execute command ```InstallUtil.exe <path to the ADImportService.exe>``` (e.g.: ```InstallUtil.exe C:\ADImportService\ADImportService.exe```)
5. Create the ```C:\ProgramData\Kentico AD Import Service\configuration.xml``` file and copy the sample [configuration](#configuration) there
6. Open the configuration file and enter all required values
7. Open Microsoft Management Console and start the ```Kentico AD Import Service```

Immediately after starting, it gets the current users and groups and adds them to Kentico, then it turns on processing of asynchronous changes. If the application fails, it informs about the event in a Windows Event Log.


### Configuration

Here is a sample configuration you can copy to ```configuration.xml``` file. 

```xml
<ServiceConfiguration>
	<Listener DomainController="FQDN or IP of Domain Controller" 
	UseSsl="false" SslCertificateLocation="Path to .cer file">
		<Credentials>
			<UserName>UserName</UserName>
			<Password>Password</Password>
			<Domain>Domain</Domain>
		</Credentials>
	</Listener>
	<Rest UserName="Kentico user name" Password="Kentico password" 
	Encoding="utf-8" BaseUrl="http://localhost/Kentico8 (use https to ebnable SSL)" 
	SslCertificateLocation="Path to .cer file" />
	<UserAttributesBindings>
		<Binding Cms="FullName" Ldap="sAMAccountName" />
	</UserAttributesBindings>
	<GroupAttributesBindings>
		<Binding Cms="RoleDescription" Ldap="description" />
	</GroupAttributesBindings>
</ServiceConfiguration>
```

### Common installation issues

If you're not able to run the service, make sure that 

- LDAP server is accessible
- REST service is accessible (try open in your browser ```www.yourdomain.com/rest/cms.user```)
- Credentials are valid
- Kentico user is able to modify users and roles
- Windows user is able to read from AD database
- Check the Windows Event log and Kentico Event log for error messages


## Acknowledgement

The project is based on code developed by [Tomas Hruby](https://github.com/TomHruby) for his [bachelor thesis](https://is.muni.cz/th/396080/fi_b/?furl=%2Fth%2F396080%2Ffi_b%2F;so=nx;lang=en) (full text of [thesis in pdf](https://is.muni.cz/th/396080/fi_b/thesis.pdf)).
