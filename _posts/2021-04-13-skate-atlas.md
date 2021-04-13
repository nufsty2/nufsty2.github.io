---
layout: post
title:  "Skate Atlas: An Interactive Skatepark Database"
date:   2021-04-13 09:26:00 -0700
categories:
---
![Skate Atlas](/assets/images/skate-atlas-logo.png)

# [Skate Atlas](http://skateatlas.rip): An Interactive Skatepark Database

## Background
I've been skateboarding on and off since I was eleven years old. I grew up skating at the [St. George skatepark](http://skateatlas.rip/skateparks/1), which is still my favorite. However, I visited my dad in Las Vegas frequently, and I'd take my skateboard wherever I went. I found a website, [Concrete Disciples](https://www.concretedisciples.com/), that has a huge database of skateparks. I used this very frequently to find local parks when I was out of town. Recently, however, it seems the site has fallen into disrepair. It's incredibly slow, and it seems like half of the pages are broken. No disrespect to Concrete Disciples. I love what they have done, and I hope they can fix their site soon.

## Skate Atlas
To me, one of the coolest moments in coding is when you have a wish list of technologies that you want to learn or use, and then one day you think of a project that you care about that involves them. For a while, I've wanted to learn how to use [Spring](https://spring.io), how to host a webapp and database on [AWS](https://aws.amazon.com/), and see what it's like to register my own domain (for the first time ever!). Skate Atlas is the project that has allowed me to do all of that! It's basically a clone of Concrete Disciples, with emphasis on using more modern technologies and making it **FAST**!

## Development
I relied HEAVILY on the [Spring Pet Clinic](https://github.com/spring-projects/spring-petclinic) sample project. This project is great because it demonstrates how to have a REST controller, MySQL database, and web ui all working together to allow CRUD operations via a web page. Skate Atlas honestly looks like a clone of this with different colors and some additional functionality, like a Google Maps fragment and a delete button. (I'm new to open-source conventions, so I didn't fork Spring Pet Clinic when I made Skate Atlas. I'll figure that out later.) Development with Spring Boot was super enjoyable and hassle-free. Spring Boot projects come with a `mvnw` script that allows you to run `./mvnw spring-boot:run` to deploy your service to `http://localhost:8080`.

## Deployment
I made a MySQL database on AWS Relational Database Service (RDS) and initialized it with the users, databases, and tables that my app needs to work correctly. Then, I packaged the spring project into a .jar and used AWS Elastic Beanstalk (EB) to deploy the app into the web. This was a kind of a tricky process, and initially the status of my EB environment always went to "Severe". Two keys that I learned during this process:
- You must set the environment variable `SERVER_PORT=5000` in your EB environment configuration for the webapp to work.
- You must have security groups configured in a way that will allow the Elastic Cloud Compute (EC2) instance (EB provisions this for you) and your RDS database to communicate with each other.

After those two quirks, everything worked and I was able to access my website! These guides are great resources for this:
- [From AWS: Deploying a Spring Boot Application on AWS Using AWS Elastic Beanstalk](https://aws.amazon.com/blogs/devops/deploying-a-spring-boot-application-on-aws-using-aws-elastic-beanstalk/)
- [From Baeldung: Deploy a Spring Boot Application to AWS Beanstalk](https://www.baeldung.com/spring-boot-deploy-aws-beanstalk)

The next step was to use AWS Route 53 to register the domain [skateatlas.rip](http://skateatlas.rip). Why the `.rip`? Two reasons: it was cheaper than other top-level domains, and "rip" is a word used in skate lingo. To "rip" is to be a consistent and good skater. A ["ripper"](http://www.skatetolive.com/lingo.html#:~:text=Ripper) is someone who rips.
- Skater 1: "Dude, it's that guy! He **rips**!"
- Skater 2: "Yeah man, he's a **ripper**!"

Anyway, via Route 53 hosted zones I just added a DNS A record that routes skateatlas.rip to the endpoint URL of my app's EB environment. And boom! That's it!

## Now What?
I pumped this all out in a weekend. Well, it was really more like two days, because that Saturday was my 25th birthday and I didn't get much done. Skate Atlas is still very much in an alpha/proof of concept phase. I have a to-do list of ideas that I still want to implement:

* [ ] Login, authentication, and permissions for admins and normal users
* [ ] Only allow admins to create, update, and delete parks, but all users can suggest changes
* [ ] Allow users to write reviews
* [ ] Reviews include a 0-5 star rating on specific quality categories: street, rails, transition, material, etc.
* [ ] Allow to search for park with any combination of attributes, not just state
* [ ] Configure the domain to use SSL so it can use https
* [ ] Make the home page more engaging and intuitive
* [ ] Configure skateatlas.rip as a mail domain, create support@skateatlas.rip or something
* [ ] Change fields in edit or create dialog to have drop-down menus
* [ ] Modify the spring CrudDatabase object to do actual MySQL queries so I can sort results more logically
* [ ] (Still on the fence about this one) Develop an integration with advertising services. I might actually be able to make some money off of this

I want to make it clear that my intention with Skate Atlas isn't to compete with Concrete Disciples or any other skatepark database out there. I want this thing to be community-driven and be a place that anyone in skateboarding can contribute to. I want it to be a tool that will help my fellow skaters out there. More than likely though, it will just something fun to contribute to and manage in my free time.