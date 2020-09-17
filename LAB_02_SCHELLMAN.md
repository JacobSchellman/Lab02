# Lab 02 Report - STIG(ging) CentOS and Windows

###### 2/c Jacob Schellman | Computer Networks and Security | 17SEP20



## Executive Summary

I was contracted on 03 September 2020 to evaluate two systems - a Windows 10 system and a CentOS 7 system - in accordance with the Security Resource Guides (SRGs) and their respective Security Technical Implementation Guides (STIGs) maintained by the [Defense Information Systems Agency (DISA)](https://www.disa.mil/). The total process from start to finish took two days, one for each Operating System evaluated. The final result was in accordance with the terms of my employment - a minimum of at least 22 arbitrary controls were evaluated on each system, and at least 12 controls on each system were fixed and are now no longer findings or are non-applicable. 



## Introduction

In this report I will highlight the methodology I took in approaching both the auditing of the systems as well as the process I used to solve 10 of the most important issues I faced. Throughout the report I will include supplemental information (screenshots, comments, etc.) as necessary to properly convey each step I took and the outcome from each action. All information not included in this Markdown document will be submitted on D2L (full checklist files) in case any further information is needed. 



## Background

Before I begin, however, it is important to detail who I am. My name is 2/c Jacob Schellman. I am a third year Electrical Engineering student at the [United States Coast Guard Academy](https://uscga.edu), and I am currently enrolled in Computer Network and Security (CNS). As a part of our class's journey in learning about system hardening, we focused on STIGs - what they are, how to implement them, and why they are applicable to you and me. 

![img](http://www.chip-architect.org/news/Northwood_130nm_die_text_720x540.jpg "An Intel Pentium 4 -- from 2003! It's crazy to think how far technology has come and how little I understand even the earliest advancements")

My passions include Computer Programming, Linear Algebra in Computer Graphics, and Computer Architecture - but anybody can have passions. Socrates once said, "I know that I know nothing." Generally I view Socrates as pretty pretentious, but that quote in particular stuck with me. I really don't know anything - I couldn't even being to explain the inner workings of a CPU to you beyond just what a quick search online could - and I really want to change that. 

That drive is why I'm here today, and hopefully that drive will help me succeed!



## Method

In carrying out this contract, I used the following tools and resources.

* [VirtualBox](https://www.virtualbox.org/) (in combinational with [Vagrant](https://www.vagrantup.com/))
* DoD Cyber Exchange's Public [STIG Viewer](https://public.cyber.mil/stigs/srg-stig-tools/)
* [Microsoft's Edge on Windows 10 Evaluation Image](https://developer.microsoft.com/en-us/microsoft-edge/tools/vms/)
* [CentOS 7-x86 64](https://www.centos.org/)

Each of these elements was critical in approaching this problem. VirtualBox and Vagrant were critical of course as they were required to 'set the scene' for our simulation. The DoD Cyber Exchange's STIG Viewer made creating checklists and viewing the controls much more approachable. Finally, of course, both images of the operating systems we examined were absolutely necessary.



## Assumptions

Several assumptions needed to be made in approaching this contract. 

First, we had to handle the fact that we weren't actually auditing and correcting a real, physical system. It is extremely unlikely that any real world company or organization would actually be running an evaluation version of Windows 10 - so we had to bear that in mind when examining some of the controls. 

Second, we had to decide for ourselves what exactly we were examining. Several of the controls I examined in the CentOS 7 STIG assumed that you were operating on Government property - which we clearly are not. I assumed, during the auditing and correction process, that I was examining a private entities devices. In that case, several controls did not apply.

Third, I had to assume that I was not only the auditor and the security engineer, but also the sysadmin in charge at whatever company or organization I was contracted from. Several controls indicate that some settings are only a finding at the intent of the sysadmin. In all cases, I assumed that anything that was up to question was in accordance with what the sysadmin had desired. In a real world scenario, that would likely not be the case.



## Summary of Actions Taken

As there are two systems in which actions were taken, we will split this into two distinct sections.

### Windows

| Vuln ID | Severity | STIG ID        | Rule Title                                                   | Status         | Comments                                                     | Finding Details                                              |
| ------- | -------- | -------------- | ------------------------------------------------------------ | -------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| V-63319 | medium   | WN10-00-000005 | Domain-joined systems must use Windows 10 Enterprise Edition 64-bit version. | Open           | Cannot be fixed in this case - evaluation edition is OS provided. Remains open. - JCS | Edition is Windows 10 Enterprise Evaluation. - JCS           |
| V-63321 | medium   | WN10-CC-000310 | Users must be prevented from changing installation options.  | Open           | Unable to fix - gpedit does not have option to disable user control over installs. - JCS | \Installer\ is missing. EnableUserControl is missing. - JCS  |
| V-63323 | medium   | WN10-00-000010 | Windows 10 domain-joined systems must have a Trusted Platform Module (TPM) enabled and ready for use. | Not Applicable | Even though we presumed that we were domain-joined in the lab handout, we have decided that we are not in class. - JCS | Compatible TPM not found. - JCS                              |
| V-63325 | high     | WN10-CC-000315 | The Windows Installer Always install with elevated privileges must be disabled. | Not A Finding  | Fixed. - JCS                                                 | \Installer\ is missing. AlwaysInstallElevated is missing. - JCS |
| V-63329 | medium   | WN10-CC-000320 | Users must be notified if a web-based program attempts to install software. | Not A Finding  |                                                              | \Installer\ does not exist. SafeForScripting does not exist. - JCS |
| V-63333 | medium   | WN10-CC-000325 | Automatically signing in the last interactive user after a system-initiated restart must be disabled. | Not A Finding  | Fixed. - JCS                                                 | DisableAutomaticRestartSignOn is missing. - JCS              |
| V-63335 | high     | WN10-CC-000330 | The Windows Remote Management (WinRM) client must not use Basic authentication. | Not A Finding  | Fixed. - JCS                                                 | \WinRm\Client\ does not exist. AllowBasic does not exist. - JCS |
| V-63337 | medium   | WN10-00-000030 | Windows 10 information systems must use BitLocker to encrypt all disks to protect the confidentiality and integrity of all information at rest. | Open           | Could not fix. "This device can't use a Trusted Platform Module." Could be fixed in group policy. - JCS | BitLocker is off on C: - JCS                                 |
| V-63339 | medium   | WN10-CC-000335 | The Windows Remote Management (WinRM) client must not allow unencrypted traffic. | Not A Finding  | Fixed. - JCS                                                 | \WinRM\Client\ does not exist. AllowUnencryptedTraffic does not exist. - JCS |
| V-63341 | medium   | WN10-CC-000360 | The Windows Remote Management (WinRM) client must not use Digest authentication. | Not A Finding  | Fixed - JCS                                                  | \WinRM\Client\ does not exist. AllowDigest does not exist. - JCS |
| V-63343 | medium   | WN10-00-000025 | Windows 10 must employ automated mechanisms to determine the state of system components with regard to flaw remediation using the following frequency: continuously, where HBSS is used; 30 days, for any additional internal network scans not covered by HBSS; and annually, for external scans by Computer Network Defense Service Provider (CNDSP). | Open           |                                                              | No HBSS software is installed by default. This is a finding. - JCS |
| V-63345 | medium   | WN10-00-000035 | The operating system must employ a deny-all, permit-by-exception policy to allow the execution of authorized software programs. | Open           |                                                              | AppLocker is not used by default. This is a finding. - JCS   |
| V-63347 | high     | WN10-CC-000345 | The Windows Remote Management (WinRM) service must not use Basic authentication. | Not A Finding  | Adjusted group policy as requested. - JCS                    | \WinRM\Service does not exist. AllowBasic does not exist. - JCS |
| V-63349 | high     | WN10-00-000040 | Windows 10 systems must be maintained at a supported servicing level. | Open           |                                                              | Version 10.0 (Build 10240). - JCS                            |
| V-63351 | high     | WN10-00-000045 | The Windows 10 system must use an anti-virus program.        | Not A Finding  | In this case, I assume that windows defender is permissible as in-house AV. - JCS | No 3rd party anti-virus solution is installed (windows defender doesn't count?). - JCS |
| V-63353 | high     | WN10-00-000050 | Local volumes must be formatted using NTFS.                  | Not A Finding  |                                                              | File system is NTFS. - JCS                                   |
| V-63355 | medium   | WN10-00-000055 | Alternate operating systems must not be permitted on the same system. | Not A Finding  |                                                              | Windows 10 is the only OS installed.                         |
| V-63357 | medium   | WN10-00-000060 | Non system-created file shares on a system must limit access to groups that require it. | Not Applicable |                                                              | Only the approved shares are listed. - JCS                   |
| V-63359 | low      | WN10-00-000065 | Unused accounts must be disabled or removed from the system after 35 days of inactivity. | Not A Finding  |                                                              | No inactive accounts found. - JCS                            |
| V-63361 | high     | WN10-00-000070 | Only accounts responsible for the administration of a system must have Administrator rights on the system. | Not A Finding  |                                                              | All accounts with Administrative access are approved. - JCS  |
| V-63363 | medium   | WN10-00-000075 | Only accounts responsible for the backup operations must be members of the Backup Operators group. | Not A Finding  |                                                              | Backup Operators was empty. - JCS                            |
| V-63367 | low      | WN10-00-000085 | Standard local user accounts must not exist on a system in a domain. | Not A finding  | Disabled all accounts - JCS                                  | Administrator, Guest, and DefaultAccount are not disabled. - JCS |

<div style="text-align:center"></div>

The exported data from the STIG Viewer is displayed in the table above. There are a few key takeaways we should recognize from this, and to best demonstrate those takeaways, I will focus on 5 key controls. 

1. The first control we're looking at is ```V-63319```. This control essentially states that if the OS installed is not 64-bit Windows 10 Enterprise edition, then this is a finding. The issue we run into is that we most clearly are not on Windows 10 Enterprise edition. 

   ![https://tendies.moe/u/ZURbHrY0oX.png](https://tendies.moe/u/ZURbHrY0oX.png)

   You might think this is inconsequential, but when you consider the features that are given up without Enterprise edition (virtualization based security Credential Guard, namely), it is clearly a concern. Having just one system exposed in a network threatens the security of every other system.

2. The second control we'll examine is ```V-63351```. This control emphasizes the importance of an antivirus installed on the system and appears to specify that it must be a third party or bundled antivirus. All Windows 10 Systems come bundled with Windows Defender - which is more than capable, especially when compared to the vast majority of third party solutions on the market. 

   ![https://tendies.moe/u/Ks0u7WxEHq.png](https://tendies.moe/u/Ks0u7WxEHq.png)

   It is important, though, to ensure that this solution is sufficient as times change. In the past, Windows Defender was absolutely not sufficient to protect your domain, and it is more than possible that that may be the case in the future.

3. The next control to examine is ```V-63349```. In my opinion, this is one of the most critical fixes that would need to be made on a real world system. The current build of Windows 10 on this computer is 10240, which indicates that this system has been unsupported since 2018. 

   ![https://tendies.moe/u/ojgXOk93Ss.png](https://tendies.moe/u/ojgXOk93Ss.png)

   It is absolutely critical to keep your Operating System up to date - there are innumerable vulnerabilities that this system is exposed to running the version of Windows that it is. While there are exceptions for "Long-Term Servicing Branches," this most likely is not the case.

4. The penultimate control will be `V-63347`. This control ensures that Windows Remote Management (WinRM) cannot use Basic authentication - in other words, it cannot use plain text passwords. Never is it acceptable to use plain text passwords in any security-sensitive sense, so it is critical that this change is made.

   ![https://tendies.moe/u/BTPdEzLqnV.png](https://tendies.moe/u/BTPdEzLqnV.png)

5. The final control examined is  `V-63367`. This is a control close to my heart because I personally have experience with it. When I was in high school, we were issued laptops. Of course, my school tried very hard to lock down these laptops in the name of both security and learning - but they left the Default Account enabled. What's worse is that the Default Account had full administrative privileges. As you might be able to imagine, it didn't take long for people to figure that out and wreak havoc on the School's network. Assuming you don't want the same thing to happen to your network, it is critical that you disable any unused local users.

   ![https://tendies.moe/u/Rta3nMH2Pd.png](https://tendies.moe/u/Rta3nMH2Pd.png)





### CentOS 7

| Vuln ID | Severity | STIG ID        | Rule Title                                                   | Status         | Comments                                                     | Finding Details                                              |
| ------- | -------- | -------------- | ------------------------------------------------------------ | -------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| V-71849 | high     | RHEL-07-010010 | The Red Hat Enterprise Linux operating system must be configured so that the file permissions, ownership, and group membership of system files and commands match the vendor values. | Not A Finding  | Verified that avahi-daemon is a default inclusion in centos and is approved. Not a finding. - JCS | 4 files were returned as a result of the first command: avahi-daemon and the daily, monthly, and weekly anacron/cron files.  The avahi-daemon is not owned by a default user, is a finding. All anacron files are by default permission settings. This is a finding. -JCS |
| V-71855 | high     | RHEL-07-010020 | The Red Hat Enterprise Linux operating system must be configured so that the cryptographic hash of system files and commands matches vendor values. | Not A Finding  |                                                              | The command returned no results. This is not a finding. - JCS |
| V-71859 | medium   | RHEL-07-010030 | The Red Hat Enterprise Linux operating system must display the Standard Mandatory DoD Notice and Consent Banner before granting local or remote access to the system via a graphical user logon. | Not Applicable | It is my assumption that most government machines use an image with all of this already ready for use. As this is a VM, I don't really see a point in hardening in this aspect - I will move past these and instead focus on other vulnerabilties that might be more severe. - JCS | This is not included in a base install. Not applicable to this system as not a government system. - JCS |
| V-71861 | medium   | RHEL-07-010040 | The Red Hat Enterprise Linux operating system must display the approved Standard Mandatory DoD Notice and Consent Banner before granting local or remote access to the system via a graphical user logon. | Not Applicable |                                                              | Again, not included by default. Not applicable to this system as not a government system. - JCS |
| V-71863 | medium   | RHEL-07-010050 | The Red Hat Enterprise Linux operating system must display the Standard Mandatory DoD Notice and Consent Banner before granting local or remote access to the system via a command line user logon. | Not Applicable |                                                              | Again, not included by default. Not applicable to this system as not a government system. - JCS |
| V-71891 | medium   | RHEL-07-010060 | The Red Hat Enterprise Linux operating system must enable a user session lock until that user re-establishes access using established identification and authentication procedures. | Not A Finding  | Followed fix text. Users now will be locked as required. No longer a finding. - JCS | No files exist in this directory. Is a finding - JCS         |
| V-71893 | medium   | RHEL-07-010070 | The Red Hat Enterprise Linux operating system must initiate a screensaver after a 15-minute period of inactivity for graphical user interfaces. | Not A Finding  | Configuration appended to existing file. No longer a finding. - JCS | No files exist. Is a finding. - JCS                          |
| V-71899 | medium   | RHEL-07-010100 | The Red Hat Enterprise Linux operating system must initiate a session lock for the screensaver after a period of inactivity for graphical user interfaces. | Not A Finding  | Lines appended as required. No longer a finding. - JCS       | No files exist. Is a finding. - JCS                          |
| V-71901 | medium   | RHEL-07-010110 | The Red Hat Enterprise Linux operating system must initiate a session lock for graphical user interfaces when the screensaver is activated. | Not A Finding  | lock_delay appended as requested. No longer a finding. - JCS | No files exist. Is a finding. - JCS                          |
| V-71903 | medium   | RHEL-07-010120 | The Red Hat Enterprise Linux operating system must be configured so that when passwords are changed or new passwords are established, the new password must contain at least one upper-case character. | Not A Finding  | Value updated to -1. Removed comment as necessary. No longer a finding.  - JCS | Value of ucredit is 1 and commented. Is a finding. - JCS     |
| V-71905 | medium   | RHEL-07-010130 | The Red Hat Enterprise Linux operating system must be configured so that when passwords are changed or new passwords are established, the new password must contain at least one lower-case character. | Not A Finding  | Value of lcredit has been updated to -1 and is no longer commented. No longer is a finding. - JCS | Value of lcredit is one and commented. Is a finding. - JCS   |
| V-71907 | medium   | RHEL-07-010140 | The Red Hat Enterprise Linux operating system must be configured so that when passwords are changed or new passwords are assigned, the new password must contain at least one numeric character. | Not A Finding  | dcredit is now -1 and is not commented. No longer a finding. - JCS | Value of dcredit is 1 and is commented. Is a finding. - JCS  |
| V-71909 | medium   | RHEL-07-010150 | The Red Hat Enterprise Linux operating system must be configured so that when passwords are changed or new passwords are established, the new password must contain at least one special character. | Not A Finding  | ocredit is now -1 and is no longer commented. Is not a finding. - JCS | value of ocredit is 1 and is commented. Is a finding. - JCS  |
| V-71911 | medium   | RHEL-07-010160 | The Red Hat Enterprise Linux operating system must be configured so that when passwords are changed a minimum of eight of the total number of characters must be changed. | Not A Finding  | difok is now 8 and is no longer commented. Is no longer a finding. - JCS | Value of difok is 5 and is commented. Is a finding. - JCS    |
| V-71913 | medium   | RHEL-07-010170 | The Red Hat Enterprise Linux operating system must be configured so that when passwords are changed a minimum of four character classes must be changed. | Not A Finding  | minclass is now 4 and is not commented. Not a finding. - JCS | Value of minclass is 0 and is commented. Is a finding. - JCS |
| V-71915 | medium   | RHEL-07-010180 | The Red Hat Enterprise Linux operating system must be configured so that when passwords are changed the number of repeating consecutive characters must not be more than three characters. | Not A Finding  | As with all other previous, has been updated in acccordance with STIG. Not a finding. - JCS | Value of maxrepeat is 0 and is commented. Is a finding. - JCS |
| V-71917 | medium   | RHEL-07-010190 | The Red Hat Enterprise Linux operating system must be configured so that when passwords are changed the number of repeating characters of the same character class must not be more than four characters. | Not A Finding  | No longer a finding as has been fixed in accordance with all previous changes. - JCS | Value of maxclassrepeat is 0 and is commented. Is a finding. - JCS |
| V-71919 | medium   | RHEL-07-010200 | The Red Hat Enterprise Linux operating system must be configured so that the PAM system service is configured to store only encrypted representations of passwords. | Not A Finding  |                                                              | Password hashes are SHA512. Not a finding. - JCS             |
| V-71921 | medium   | RHEL-07-010210 | The Red Hat Enterprise Linux operating system must be configured to use the shadow file to store only encrypted representations of passwords. | Not A Finding  |                                                              | Output is as expected. Not a finding. - JCS                  |
| V-71923 | medium   | RHEL-07-010220 | The Red Hat Enterprise Linux operating system must be configured so that user and group account administration utilities are configured to store only encrypted representations of passwords. | Not A Finding  |                                                              | Output is as expected. Not a finding. - JCS                  |
| V-71925 | medium   | RHEL-07-010230 | The Red Hat Enterprise Linux operating system must be configured so that passwords for new users are restricted to a 24 hours/1 day minimum lifetime. | Not A Finding  | Fixed in /etc/login.defs. No longer a finding. - JCS         | PASS_MIN_DAYS is 0. This is a finding. - JCS                 |
| V-71939 | high     | RHEL-07-010300 | The Red Hat Enterprise Linux operating system must be configured so that the SSH daemon does not allow authentication using an empty password. | Not A Finding  | It appears this is not a finding by a default installation of centos. No change necessary. - JCS | Line is commented. Not a finding. - JCS                      |
| V-71967 | high     | RHEL-07-020000 | The Red Hat Enterprise Linux operating system must not have the rsh-server package installed. | Not A Finding  | RSH is uninstalled. No longer a finding. (I thought we were gonna do something cool with it, not just uninstall it D:) - JCS | RSH is installed. This is open. - JCS                        |


As with Windows 10, the exported data from the STIG Viewer is shown in the table above. One major difference between Windows 10 and CentOS, however, is the number of controls examined and the results of those controls. We will again pick 5 major controls to better understand this.

1. The first control we will examine is one of the most critical. ```V-71939``` ensures that nobody can SSH into this computer with an empty password. While certainly it is best practice to never have that empty password in the first place, an extra layer of security never hurts and often saves countless hours and dollars. Fortunately, the installation of CentOS I used already had this control taken care of.

   ![https://tendies.moe/u/WiZRxarzGP.png](https://tendies.moe/u/WiZRxarzGP.png)

2. The second control we will examine is ```V-71849```. This control seeks to ensure that there is no misconfiguration in which users and groups have access to which files and directories. While auditing, I noticed that there were several directories and files in which all users, regardless of who they were, had permission to read from: ```/var/run/avahi-daemon```, ```/var/lock/iscsi```, and ```/var/lock/iscsi/```. 

   ![https://tendies.moe/u/Cyj5pRezKf.png](https://tendies.moe/u/Cyj5pRezKf.png)

   Fortunately, these are expected and do no qualify as a finding. It is always important to check to ensure that, where permissions are granted, they are expected.

3. The third control we will examine is ```V-71855```. This control is important because it helps guard against any malicious installation of the operating system. In some cases, disk images include malware or otherwise unwanted changes to a base operating system that can easily compromise the security of the network the computer is added to. By verifying the cryptographic hashes of intrinsic file systems and commands, we can verify our installation as secure.

   ![https://tendies.moe/u/4LUKbvQqsZ.png](https://tendies.moe/u/4LUKbvQqsZ.png)

4. The fourth control, just like the fifth, is actually a serious of controls that we will examine at once. ```V-71859```, ```V-71861```, and ```V-71863``` all essentially get at the same idea: that we need to display a variety of messages to the user indicating that they are on a government system. However, I went into this contract under the assumption that I was working for a private entity and, as such, need not implement these changes. This is an important lesson to take away - that discretion is important and that STIGs are not the end all be all when it comes to security and sensibility.

   ![https://tendies.moe/u/DqfXy5IbOr.png](https://tendies.moe/u/DqfXy5IbOr.png)

5. The final set of controls we will examine are ```V-71891```, ```V-71893```, ```V-71899```, and `V-71901`. Each of these controls is specific to the GNOME Linux shell and directly deal with locking a user when they have idled for more than a certain period of time. This is a key concept that isn't seen much in the SRGs that have been examined so far - security isn't just a part of what goes on behind the GUI, it's also what happens right in front of it. 

   ![https://tendies.moe/u/FjcehRgG51.png](https://tendies.moe/u/FjcehRgG51.png)



## Highest Impact Controls

In the summary of actions taken, I dove into 15 controls that I thought highlighted something important about each system I audited and repaired. Of those 15 controls, I have identified 5 that I believe are the most mission-critical for maintaining a secure and functional network.

The first control and the most important in my mind is the Windows 10 `V-63349` control. It cannot ever be fully secure by nature of the fact that it no longer is receiving updates and maintaining a system which has no chance of ever being fully secure (unless air-gapped) is a massive liability. There is a reason the control requires a supported version of whatever Operating System you are running - and it's a very good reason. If I could only choose one control to implement of all the ones I examined, it would be this one.

The second control I find to be of critical importance is Window's `V-63335`. Storing passwords in plaintext is a surefire way of crippling your security. The only logic I have heard in defense of plaintext storage of data is the idea that, if an attacker is already far enough to access your databases, it's too late - and that doesn't even make sense. Why throw everything out the window because of one failure? It's poor practice and irresponsible. Even in `V-63335`'s case, this does not make sense. It strikes me as odd that Basic authentication would even be the default in the first place!

The third control that stands out to me is a set of controls for CentOS, ```V-71891```, ```V-71893```, ```V-71899```, and `V-71901`. Just as I stated previously, security is a 24/7 endeavor. Just as was the case with [stuxnet](https://en.wikipedia.org/wiki/Stuxnet), often times the biggest threat to a network's security isn't inside the computer, but instead sitting in front of it. Preventative action is the only approach that we can take here, and we should be as aggressive as possible in that regard.

The fourth control with a high impact potential is Window's `V-63367`. This is very similar to the idea as with CentOS's `V-71849` control. In both cases, the STIG highlights that preventing users from accessing data outside of their normal operating area is a key idea behind security. With Windows, if a default user is still active on a domain, _anybody_ can access that account. On CentOS, _anybody_ can access the files that might have improper permissions set. As with every other concept, all it takes is one mistake to compromise the security of an entire network.

The final control I will talk about is one that I didn't mention in the previous section - `V-71919` from CentOS. As with several other CentOS STIGs, this is actually one of several controls that affect the same concept. However, I wanted to highlight here the importance of ensuring that your passwords are not only secure in structure (length, complexity, etc.), but also in the way that they are stored. It is the same idea as with Window's `V-63335`, but perhaps with a more direct line of reasoning. Whereas with WRM it was only one access point, if CentOS is misconfigured to store passwords in a way that is insecure or easily cracked, then it can have a devastating impact.



## Conclusions

There are a number of lessons to be learned from the implementation of the Windows 10 and CentOS 7 STIGs that have been completed thus far. 

I believe the most important takeaway is that there is a lot that goes into securing just one system. Definitely there is more than what one person can do alone. I believe that this enforces the idea that cyber security is a team effort - everybody needs to be on board in order in order to ensure the overall security of the network. All it takes is one teammate to not do their job for just one second and all of the effort that went into hardening is for naught.

I also understand now the sheer amount of intricacies that go into maintaining a hardened network. Each and every day there are countless updates and changes made to software on each and every system in the world. All it takes is for one change to go awry or one change to revert a previously hardened setting, and you can potentially have a very vulnerable network. It's a never ending battle, and this is just the first step towards success.

