---  
title: "What in this world is Kerberos and setting up a Kerberized environment with Ansible"
comments: true
category: ["configuration management","ansible", "authentication"]
---  

<p align="center">
  <img src="https://media.giphy.com/media/jp2KXzsPtoKFG/giphy.gif"/>
</p>

- [What is Kerberos?](#what-is-kerberos)
- [Kerberos Terminology](#kerberos-terminology)

# What is Kerberos?
  * Authentication Protocol
  * Client/Server Architecture

# Kerberos Terminology
  * An authentication administrative domain is called a **Realm**. A realm establishes a boundary - administrative boundary
  * All principals are assigned to a specific realm
  * Identities are called **principals**
    * Every party(user or service) that participates in the Kerberos auth protocol requires a principal to uniquely identify itself.
    * Principal Categories:
      * User Principals: Regular Users (User Principal Names)
      * Service Principals: Service that a user needs access to(Service Principal Names)