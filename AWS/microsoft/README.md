# Microsoft related AWS Notes

## Windows file server
A Windows machine that shares folders over the network using SMB. Can be used to provide file storage for Windows-based applications or users. Can be deployed on EC2 instances or using AWS FSx for Windows File Server.


- SMB share vs NTFS permissions


## Active Directory (AD)

Central identity management system for Windows environments. Can be deployed on EC2 instances or using AWS Managed Microsoft AD.

## FSx for Windows File Server

A managed Windows file server in AWS. You access it via SMB like \\server\share. It can join Active Directory so permissions work like on-prem.