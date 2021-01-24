## Mapping Your Hashnode Blog to Your Root Domain In Google Domains Without Cloudflare

You just signed up for your Hashnode blog and now you want to link your custom domain in Google Domains, You might ask how can I do that? Well, in this article you will learn how to do that without using an external (or additional) DNS provider.

### Basically we will be doing two things

- [Configuring Your Blog Hashnode](#on-hashnodecom) 
- [Configuring Your Domain in Google Domains](#now-on-the-domain-registrar-site)


*Let's start!
*

#### On Hashnode.com

You do the following:

- #### Go to your blog **settings**
![Hashnode dashboard](https://cdn.hashnode.com/res/hashnode/image/upload/v1611341267101/yBZtFdi_u.png)

- #### Then Click on the **Domain** option on the sidebar
![Hashnode domain option in dashboard](https://cdn.hashnode.com/res/hashnode/image/upload/v1611341657790/FTNb0fnZu.png)
In here in the input box you type in your domain but must be preceded with `www.` why you might ask? because we need a sub-domain, and Google Domains only supports CName records for/with sub-domains and can't be used with root domains yet. *Google Domains doesn't support CName Flattening.

So you input value should be like `www.[your domain].[your TLD]`

You might say I can used an A-Record (linking the DNS) while thats true, but that breaks two of the most important features of using Hashnode for your blogging needs, and they are the following

  - Hashnode Global CDN
  - Edge Caching

More on those two [here](https://sandeep.dev/how-i-built-a-cdn-for-our-multi-tenant-app-within-a-day)

----

#### Now on the Domain registrar site

Here Google Domains more specifically, we do the following steps after selecting your domain:

- #### Go to the **DNS** section of your domain

![Google Domains DNS Section](https://cdn.hashnode.com/res/hashnode/image/upload/v1611343540277/d1CwvLCTI.png)
Here in the custom resource records area we add a new record with type of **CName** with the name of `www` and value (or data) of `hashnode.network`. which in my case the values were like
```
Name: www,
Type: CNAME,
TTL: 1h,
Data: hashnode.network.
```

After you hit add on your new resource record,You have done the initial steps, you might get a message that say you changes might propagate within 24h ~ 48h but in most cases it would take less than a few minutes.
Now you can check at 

`https://www.[your domain].[your TLD]`

And you should see your Hashnode blog, but what if we tried the root domain?

`https://[your domain].[your TLD]`

It won't work because we only mapped the sub-domain `www`. And we will fix this in the next step.

- #### Then Setup a **Redirect** from the root domain to the sub-domain

This is a **trick**! by redirecting the the root domain to our sub-domain here, we fix the site not loading when we point at our root domain. Let's start In the **DNS** section of our domain in Google Domains in **Synthetic records** area we need to add a new record

![Google Domains Synthetic records](https://cdn.hashnode.com/res/hashnode/image/upload/v1611345286025/d-Q02iLcr.png)
In here we add a new record of type `Subdomain forward` in the subdomain input we insert `@` which means the root and in the destination we insert our blog url like `https://www.[your domain].[your TLD]` and you make sure to enable `forward path` if you need it. In my case the values were the following,

```
type: Subdomain forward,
subdomain: @,
destination: https://www.rawand.dev,
forwardPath: enabled,
ssl: enabled,
redirect: temporary(302)
```

After you hit add, it might take a few minutes for your changes to take effect *depending on your registrar. later when you try your root (main) domain again it should load just fine as it will be automatically redirected to your sub-domain by the browser (client app).


### Now you have your Hashnode blog up and running fine `without the help of any extra DNS provider.`

if you have any questions, you can leave a comment or you can [contact me](https://rawand.dev/contact)!

Now start blogging!!