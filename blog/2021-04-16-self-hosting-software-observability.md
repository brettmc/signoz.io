---
title: Self-hosting software & why it may be worth considering again now
slug: self-hosting-software-observability
date: 2021-04-16
tags: [self-hosting]
author: Pranay Prateek
author_title: SigNoz Team
author_url: https://github.com/pranay01
author_image_url: https://avatars.githubusercontent.com/u/504541?v=4


---
Not all industries are the same in terms of the sensitivity of data they handle. And as you mature as a company, you need to be more careful of how you handle your critical data. With the advent of modern cloud native technologies & Kubernetes, on-prem software is more viable now
<!--truncate-->

I can hear your mind wheels spinning - 

> *What’s this blasphemy? Isn’t SaaS the end of all evils - why are you asking me to think about keeping data in your infrastructure?*

Well, well - allow me to explain. SaaS **IS** the greatest thing after sliced bread, if you are a small startup. You don’t care about where your data is, privacy and security is not something which you are concerned about right now. You just want to move fast and break things! 

But not all companies are born equal, and not all industries are the same in terms of the sensitivity of data they handle. And as you mature as a company, you need to be more careful of how you handle your critical data.

For example, if you are a fast growing fin-tech company working with a number of banking APIs and enabling hundreds of financial transactions daily, you better keep your data within your infra or at least within the same cloud where the rest of your data is. You can’t use a SaaS service which has its servers in the USA while you are in India, otherwise your auditors will come after you. Especially if it contains data which can potentially have customer information like traces or logs. 

In the observability domain, traces and logs are something which you don’t want to send outside. Unless you have great PII scrapers running before sending data outside, and well we all know it is not easy to create PII scrapers which work with 100% accuracy. So even if you have PII scrapers, why do you want to take the risk of some tiny bit of customer data leaking - and you inviting the fury of your legal and audit teams.

![](/img/blog/2021/04/onprem-monitoring-tracing.jpg)60% of companies run monitoring & logging within their infra ([CNCF Report](https://www.cncf.io/wp-content/uploads/2020/11/CNCF_Survey_Report_2020.pdf))
There are few other reasons also which should at the very least encourage you to think about this option

1. Increase in privacy laws like GDPR/CPRA
2. Flexibility of configuration as you are not crossing the Internet boundary every time
3. Data egress cost
4. Data breaches
5. More control - Some teams just like more control of the software running, not depend upon scheduled maintenance & downtimes of SaaS products. What if you have a critical release coming up, and there is a downtime

Let us examine few of these in more detail

- **Privacy Laws** - The *General Data Protection Regulation (GDPR)* came into effect in May 2018 and has been an essential step in strengthening citizens’ fundamental rights in the current Digital Revolution, and monitoring businesses, and preventing these companies from misusing data for their capital gains which puts the user at risk.

*California Privacy Regulation Act (CPRA)* is another privacy law which will impose more privacy requirements around sharing user data with 3rd party vendors. From 2023, users can opt-out of their data being shared to 3rd party vendors ( including vendors like DataDog, Google Analytics, etc.). If you are using a 3rd party monitoring tool, you have to scrub your observability data before sending it to them.

Since, you won't have complete visibility into what is happening in your software infra now, how will you debug it when a critical issue comes & management is breathing down your neck to fix it - like yesterday?

- **Data breaches** - Cloud vendors are a honeypot for security breaches, as hacking a single SaaS vendor will give hackers access to critical data of many companies. You don’t know if your data also gets breached because hackers hacked a SaaS provider for accessing another of their customers. Many times there’s no clear isolation between data of different companies.

If you are using 100 SaaS vendors, and each has only 0.5% chance of having a security breach, then you have 1 - (0.995)^100 = **~40%** chance of having a security breach. 

      Are all your SaaS vendors equally well secure?

- **Data egress cost **- This is something which many engineering teams are not aware about, as this is just a line item in your AWS/GCP bill which few people give attention to. But if you are sending huge amount of monitoring/logging data to a SaaS vendor, this line item will soon start rearing its head. 

		AWS charges for transfer that send data out over the internet are billed at   region-specific and tiered data transfer rates. If you are sending 10 TB in a month to Internet, just this would add **900 USD** to your monthly bill.
![](/img/blog/2021/04/Screenshot-2021-04-16-at-7.01.00-PM.jpg)AWS data transfer pricing ( [link](https://aws.amazon.com/ec2/pricing/on-demand/) )
Grant Miller, CEO of Replicated recently wrote an interesting [blog](https://techcrunch.com/2018/06/17/after-twenty-years-of-salesforce-what-marc-benioff-got-right-and-wrong-about-the-cloud/) on why modern on-prem delivery model is a much better software delivery model than SaaS. 

If you think about it, SaaS was a welcome change from the world of legacy on-prem software deployment in early 2000s. You would need to buy costly hardware, the annual licenses to start running with your software and then employ consultants to actually get the software up and running. The whole process would easily take 3-6 months - and this is when everything  runs on time. Then there would be annual ritual of upgrading the software which would be another pain in the a***.

Understandably, SaaS was a welcome change in this world. 

But with the coming of new cloud native technologies and Kubernetes, this process has become much more simpler. You can install a software today by running

*kubectl deploy*

and to upgrade the software you just need to update the Helm charts and deploy again.

And when this gives you advantages of privacy, security and more control as discussed about, why not at least give some serious thought to it.

---

If you need more social proof to give more thought into this, here's  Elon Musk tweeting about how Tesla **ONLY **uses *"internal & open source software"*

> Tesla is using only internal & open source software & operates Bitcoin nodes directly.
> 
> Bitcoin paid to Tesla will be retained as Bitcoin, not converted to fiat currency.
> &mdash; Elon Musk (@elonmusk) [March 24, 2021](https://twitter.com/elonmusk/status/1374619379929772034?ref_src=twsrc%5Etfw)

---

If the above made any tiny bit of sense, check out SigNoz, which is an open-source, self host-able alternative to application monitoring products like DataDog. Give it a whirl and let us know what you think!
[

SigNoz/signoz

Open source Observability Platform. 👉 SigNoz helps developers find issues in their deployed applications & solve them quickly - SigNoz/signoz

![](https://github.githubassets.com/favicons/favicon.svg)SigNozGitHub

![](https://repository-images.githubusercontent.com/326404870/e961a900-63c9-11eb-83f6-02913cf1b477)
](https://github.com/SigNoz/signoz)