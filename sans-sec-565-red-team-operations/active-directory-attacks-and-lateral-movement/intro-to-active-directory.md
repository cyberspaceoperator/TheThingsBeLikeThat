# Intro to Active Directory

## Components

* AD is basically a database
* Schema - Contains definitions of every object that can be created in AD
* Indexing & Querying - Provides a mean to search specified records in the AD database
* Global Catalog - Contains information about every object in a forest
* Replication Service - Distributes information across domain controllers
  * <mark style="color:red;">Most interesting for red teamers</mark>

## GPO

* Allow admins to centrally manage settings
* Starter Group Policy Objects allow creating a "template" GPO
* Local Group Policy Objects only applies to local computer and users
* Non-Local Group policy objects - The "standard" GPO, applies to objects (users and computers) in AD and are distributed from the domain controllers

<figure><img src="../../.gitbook/assets/image (120).png" alt=""><figcaption></figcaption></figure>

## Trees & Forests

**Trees** - Collections of related domains (<mark style="color:red;">NOT SECURITY BOUNDARY</mark>)

**Forests** - collections of trees that trust one another (<mark style="color:green;">SECURITY BOUNDARY</mark>)

Domain admins are defined on a domain level (dev.draconem.corp)

Enterprise admins are defined in the root domain at a forest level (draconem.corp and thunderbird.corp)



## Trusts

* Used to allow autherntication between multiple domains
* By default, microsoft creates
  * Parent-child trust (two-way transitive) when a new domain is added in a tree
  * Tree-root trust (two-way, transitive) when a new tree is added in a forest

One way trust - Only one domain will be able to access resources in the other, but not the other way around

Two-way trust - standard

Authentication - NTLM or Kerberos

<figure><img src="../../.gitbook/assets/image (121).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (122).png" alt=""><figcaption></figcaption></figure>

