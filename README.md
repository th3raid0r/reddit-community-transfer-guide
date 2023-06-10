# reddit-community-transfer-guide

This guide will walk you through the process of setting up a Lemmy instance and transferring a Reddit community over. 

## Table of Contents
1. [Secure a Domain](#secure-a-domain)
2. [Attach Domain to Email Service](#attach-domain-to-email-service)
3. [Configure Cloud/VPS Provider](#configure-cloudvps-provider)
4. [Configure the Lemmy Instance](#configure-the-lemmy-instance)
5. [Configure DNS](#configure-dns)
6. [Reddit Blackout Day](#reddit-blackout-day)
7. [Create ToS and Community Guidelines](#create-tos-and-community-guidelines)
8. [Begin the Scaling Process](#begin-the-scaling-process)
9. [Advertise the Lemmy Instance](#advertise-the-lemmy-instance)

## Secure a Domain

Now, here comes the fun part. Choosing a domain name can be a wild ride, much like picking a username for your favorite MMO. You could go the traditional route with a `.com`, `.net`, or `.org`, or you could channel your inner hipster and go for something a little more "in vogue" like `.social`, `.community`, `.online` or even `.lol`. 

Remember, your domain name is like your first impression in an online date - it tells a lot about you and your community. So pick something snazzy and easy to remember. Or, if you want to have some fun, choose something completely nonsensical and irrelevant. Who wouldn't want to join a community hosted at `wombat-pizza-disco.social`?

But in all seriousness, your domain name should reflect the community you're catering to and remember it's the front-facing name for all your future marketing efforts. You're going to be plastering this on bus stops, writing it on post-it notes in public places, printing it on your dog's sweater. Make it count.

Here are a few tips to consider while you're playing the Domain Name Hunger Games:

* **Keep it short & simple:** Like the best passwords, the best domain names are short, sweet, and hard to forget. Only this time, you *want* people to remember it.

* **Avoid unusual characters:** Numbers and hyphens can be confusing when spoken aloud, like "Wait, is that 'three' or 'three' spelled out?" So unless you enjoy making your community members' lives more complicated, keep it simple.

* **Local or global:** If your community is specifically for Tucsonians, something like `tucson.social` could be perfect. But if you're aiming for global domination, you might want something less geographically bound.

After you've finally landed on the *perfect* domain name, head over to a domain registrar like Namecheap, GoDaddy, or Google Domains, and lock that puppy down. If you're lucky, you won't be engaged in a bidding war with another passionate group of internet citizens who are *also* looking to start a Lemmy community for fans of 18th-century Mongolian throat singing. 

So remember, securing a domain is not just a task, it's a rite of passage. Welcome to the big leagues, kid.


## Attach Domain to Email Service

Once you have your shiny new domain in your pocket, the next step is to attach it to an email service that provides SMTP access. This may sound like you're creating a Frankenstein's monster of internet services, but it's actually quite straightforward. 

Let's use SendGrid as our example here. Why SendGrid, you ask? Well, they're well known, they're reliable, and most importantly, they have a generous free tier that's enough for small communities. If you are starting a subreddit for every human on earth, you may need to look elsewhere.

Here's a basic rundown of how you'd set up your domain with SendGrid:

1. **Create a SendGrid Account:** Head over to [SendGrid](https://sendgrid.com/) and sign up for an account. You'll have to provide some basic information about yourself and your community, but don't worry, they don't bite.

2. **Verify Your Domain:** This involves adding a few DNS records to your domain. It's a bit like a secret handshake between SendGrid and your domain to confirm they're on the same team. SendGrid provides clear instructions on how to do this.

3. **Create an API Key:** Once your domain is verified, you'll need to create an API key. This is essentially a secret password that your Lemmy instance will use to send emails through SendGrid. 

4. **Configure Lemmy with Your API Key:** Finally, you'll need to configure Lemmy to use this API key. This usually involves updating your Lemmy configuration files with your SendGrid API key and SMTP server information.

A quick reminder to not share your API key or SMTP server information with anyone. It's like your toothbrush or your underwear – it's personal and it's yours. If you accidentally commit it to GitHub, it's a bit like accidentally going to the supermarket in your underwear. So double-check your commits!

Remember, email is the primary communication medium for account verification, password reset, notifications, and newsletters. So, be sure your email system is up and running before you launch your community.

## Configure Cloud/VPS Provider

The beauty of Lemmy is that it's designed for decentralization, to be spread out across small instances rather than conglomerated into one mega instance. This is fantastic news if you don't have a giant server farm at your disposal and want to run your community out of your apartment's linen closet. In other words, you don't need a tech behemoth's resources to create a vibrant Lemmy community. With enough determination (and a stable internet connection), you could run your instance on a proverbial "toaster" – assuming your community is small and doesn't mind a bit of extra "toastiness".

However, for the purpose of this guide, let's assume you're looking to use a VPS (Virtual Private Server) that is directly exposed to the internet. This is a good middle ground between the "toaster" and the tech behemoth. A VPS offers more power and stability than most home servers but is cheaper and easier to manage than a full-fledged cloud solution. 

Cloud environments like AWS, Google Cloud, or Azure provide scalability and are great options if you expect your community to grow significantly or quickly. But for most small to medium communities, a VPS should suffice.

Here are the basic steps to setting up a Lemmy instance on a VPS:

1. **Choose a VPS provider:** There are many to choose from, but some popular ones include DigitalOcean, Linode, and Vultr.

2. **Select a server package:** You'll need a package with at least 2 CPU cores and 8GB of RAM to start with. You can always upgrade later if needed.

3. **Choose an operating system:** Lemmy is designed to run on Linux, so something like Ubuntu or CentOS is a good choice.

4. **Create SSH Keys:** You'll need to create a pair of SSH keys – a public key that you'll give to your VPS provider, and a private key that you'll keep secret. SSH keys are like a set of futuristic, cryptographic walkie-talkies that let you communicate securely with your server. For more information, Google "How to create SSH keys".

5. **Configure your server:** Once your VPS provider has your SSH public key, they'll spin up your server. You can then log in using your private key and start configuring your server for Lemmy.

Remember, this guide is a very high-level overview. Server configuration is a big topic, and there's a lot to learn. So if you're not sure about something, don't panic. Take a deep breath, open up your favorite search engine, and look up terms like "How to configure a VPS for Lemmy" or "VPS setup for beginners". The internet is filled with tutorials, guides, and friendly forums where you can ask questions and get help.

## Configure the Lemmy Instance

Congratulations! You've reached a crucial step in setting up your Lemmy instance. Hopefully, you've successfully set up your VPS, installed your chosen operating system, and remembered to mount the volumes you've created (you did remember, right?). Now, let's roll up our sleeves and get Docker and Docker Compose installed on your server.

Keep in mind, these instructions are for Ubuntu/Debian-based instances. If you're using another Linux distribution, don't worry, the process should be quite similar. You might need to google the package names for your specific distro.

### Install Docker

Run the following commands in your terminal to install Docker:

```bash
sudo apt update
sudo apt install apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
sudo apt update
sudo apt install docker-ce
```

### Install Docker Compose

Next, let's install Docker Compose. Execute these commands:

```bash
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```

With Docker and Docker Compose up and running, your server is starting to resemble a Lemmy-friendly environment. The next step is to fork the Lemmy repository, along with the related repositories. Head over to [Lemmy's GitHub page](https://github.com/LemmyNet/lemmy), click the "Fork" button in the upper right, and clone the forked repository to your server.

Next, we need to talk about SSL certificates. Why? Because the Internet of 2023 loves encrypted traffic, and so should you. Two convenient (and free) options for this are Lets Encrypt and Cloudflare Origin Certificates. Pick the one that suits you best, follow their setup instructions, and get yourself that shiny new certificate.

With that done, you're ready to unleash your Lemmy instance. Navigate to the directory where your `docker-compose.yml` file lives and spin up your Docker stack:

```bash
cd /path/to/your/docker-compose.yml
docker-compose up -d
```

At this point, your Docker stack should be up and running.

**IMPORTANT!**

Before we move on, let's talk about passwords. No, seriously, let's talk about passwords. Your default passwords are just that – defaults. They're placeholders, like the dummies in a store window. They're not supposed to be the real deal. So, for the love of all that is secure, **CHANGE YOUR DEFAULT PASSWORDS!**

Store them in a password vault (you have one of those, right?). If you don't, consider using a service like Bitwarden. It's free, secure, and it will save you from the headache of forgetting or losing your passwords. You wouldn't leave your front door wide open when you leave the house, would you? It's the same principle here. 

So, remember: 

- **YO! CHANGE! THE! PASSWORDS!**
- Store them in a safe place (like Bitwarden).

Coming up, we'll get your domain to point at your brand new Lemmy instance by delving into the wild world of DNS configuration.


## Configure DNS
Once your Lemmy instance is set up and running, you will need to configure your DNS provider to point your domain to your Lemmy instance's public IP. If you are using a load balancer, you will need to point your domain to the CNAME of the load balancer.

## Reddit Blackout Day
Ideally, you will want to have your Lemmy instance ready by Reddit's blackout day for the best chance of success. 

## Create ToS and Community Guidelines
As one of your first actions on your new Lemmy instance, create an open Terms of Service and Community Guidelines page. Make this the first post on your instance and ask for feedback from your community to improve and amend these documents as necessary.

## Begin the Scaling Process
Once your Lemmy instance is running and has an active community, you can start the process of scaling. 

1. Begin by moving your database to a redundant, high-availability cluster.
2. Then, scale out your image/video service layer using "pictrs".
3. Lastly, scale out your lemmy-ui services if needed.

## Advertise the Lemmy Instance
Throughout this process, and especially after your Lemmy instance is up and running, start advertising it to the local community and beyond. 

Use both online and offline methods for this. Consider viral marketing and guerrilla marketing techniques to save costs. Advertise at bus stops and other common areas where people wait. 

Remember that the success of your community largely depends on attracting an active and engaged user base, so don't neglect this important step.

