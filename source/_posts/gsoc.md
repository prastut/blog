---
title: Matchmaking Module for Teem
subtitle: Under Google Summer of Code program, 2016.
cover_index: /images/posts/gsoc_cover.jpg
live_preview: https://teem.works/
code: https://github.com/P2Pvalue/teem-tag/tree/master
date: 2016-07-30 12:49:32
project_duration: March - August 2016.
cover_detail: /images/gsoc/intro.jpg
googleslides: 10qUKU-s42dNnkpIa4DrUmlhs7TyneeZeOT4i01StzVo
---


I am pleased to announce that I got selected for the prestigious Google Summer of Code '16 with [Berkman Klein Center for Internet and Society](https://cyber.law.harvard.edu/), Harvard University. Amongst the various open source projects by Berkman Center, I was selected for [Teem: a P2PValue collaboration app](https://teem.works/). This blog post details my work throughout the 5 months that I have been working with Teem members. 

## About Teem

Teem is an app focused on increasing the participation and sustainability of commons-based peer production communities (e.g. Wikipedia, free software, Arduino). The model can also be applied to other open online communities (networks, open organizations) or even social movements (social centers, collectives). The 1-9-90 rule states that the collaboration on projects like Wikipedia, 90% of the participants of a community only view content, 9% of the participants edit content, and 1% of the participants actively create new content. After doing intensive social research and prototype testing, the main needs of the different roles within a community and the tools they typically lack (related to management and internal organization, listing the subprojects available and the needs of each) have been recognized. 

![About Teem](/images/gsoc/about_teem.jpg)



The app is grounded on these findings to reduce the frustrations of all participants and increase participation (90s=>9, 9s=>1), while providing a kind-of project management tool for communities (but informal/liquid/open to fit the context) together with a work-space with collaborative edition (like google-docs) and a group chat (like a whatsapp/telegram group). 

![Teem USP](/images/gsoc/collabview.jpg)

Links:

* Current web-app: http://teem.works. 
* Android app encapsulating the web-app: http://tiny.cc/teemapp 
* Code: https://github.com/P2Pvalue/teem

## My role 

* Problem Statement:
When people start using a tool such as Teem, they come with a set of skills and interests. On the other hand, community projects have needs to fulfill, i.e. they have gaps or missing skills that are slowing down their development. I wanted to ease this very challenge of matching them, connecting newcomers with communities appealing to them. 
My initial proposal : [GSoC Teem Matchmaking Proposal](https://docs.google.com/document/d/1DwtxhYupN_e8bX13vntU7csiP4hrbZq4MftBJGhE6v0/edit?usp=sharing). 

* Discussion (Community Bonding Period): 
During the community bonding period, the Teem project members and I got down on Hangouts to discuss the proposed solutions. Every member had to have an open mind and throw around crazy ideas. We were simultaneously collaborating on Google Docs, and jotting down our ideas. This type of planning was new to me, and I followed up by integrating such workflow in my college projects too. Links to our discussion: 
	* [Matchmaking Creative Session](https://docs.google.com/document/d/1CXg_3Mpr87PD6t4ThH-zJiDy3XIqxgm_dCZNtXiK-FQ/edit?usp=sharing).
	* [Updated Matchmaking Proposal](https://docs.google.com/document/d/1LvVBdYovyf2vtXEOzq3OtNOdbfOCml6tqJxyG4ZhKe4/edit?usp=sharing).

* Miscellaneous:
   * [Small Bugfixes](https://github.com/P2Pvalue/teem/commits?author=prastut) were made during the bonding period to get acquainted with Teem's codebase. 
   * Our internal communication was shifted to Gitter. Most of our discussions can be found here: [Teem Matchmaking Gitter Channel](https://gitter.im/P2Pvalue/teem-matchmaking).


## Final Solution

![Solution](/images/gsoc/matchmaking.jpg)

Through the various discussions above, we formalized a matchmaking module. The proposed task flow was:

 * Introduce a new route for exploration called interests. Users without even signing up can click on this. 
 * They would be presented with interests: tiles with categories like music, technology, culture etc.  
 * Once they select some of their interests, they would be taken to matchmaking screen. Here the user will be presented with the list of communities that correspond to user interests. 
 * The communities will also be tagged through their content, and the users can also further make their search specific by clicking on the tags.

Mockup of the above workflow:
![Workflow](/images/gsoc/matchmaking_mocks.jpg)


Through the above feature-set, the friction the user feels in terms of exploration on their own will drastically get reduced. Usability studies were also conducted to validate our design hypothesis: 
* [Mobile Prototype](https://popapp.in/w/projects/573c352f5dadb3423eba1c7f/preview/573d807cefecdecb057c2a50?transition=none&t=1471383993798).
* [Usability Experiment conducted with 2 college friends](https://docs.google.com/document/d/1DfvKFUM0av9GngK-hunPttGycYIui95A2wMsnJa-bjE/edit?usp=sharing).


## Let the coding begin!

The above solution was broken down into 2 parts:

### Developing an automatic keyphrase extraction system:

**Objective:**
Automatic analysis of the textual content of communities and teams in the platform to get valid and important tokens. By analyzing the data from the communities, we can generate a map of interests. The user can be onboarded more easily through the idea of having to select their interests and then being directed to relevant communities. Future goals of this module is to become a full fledged Machine Learning + Natural Language Processing tasks related to the project.

**Architecture:**
The module is written in [Python](https://www.python.org/) and makes extensive use of [NLTK](http://www.nltk.org/). [Flask](http://flask.pocoo.org/) is added on top to provide webserver support. This entire module is dockerised into [Teem-Tag image](https://hub.docker.com/r/p2pvalue/teem-tag/). The image is used in a docker container which integrates with the [SwellRT](https://github.com/P2Pvalue/swellrt), the full-stack backend framework for Teem. SwellRT makes a HTTP dispatch to Teem-tag Daemon server which receives the text, processes it, and makes a POST request containing the tags back to SwellRT to update the teem/community object. Following is a schematic diagram for the same: 

![Architecture](/images/image07.png)

Code for this module: https://github.com/p2pvalue/teem-tag. 

This module received the following mentions in various contests apart from GSoC:
* I presented this module as a speaker in Pycon, India's Annual Python Conference. [Pycon'16 Slides](https://docs.google.com/presentation/d/1VLjU2MQnB3GUw7p4z_Q6olFdYoW2q-LTsE8OUFZqrgI/edit?usp=sharing).
* This module was extended to incorporate image processing support using Tensorflow. It was awarded the first and the second prize in [SwellRT's free software contest](https://swellrt.org/contest).

### Developing the Matchmaking screens:

**Objective**
The screens had to be integrated with the current design philosophy of Teem. I took some time to read and get used to the design thinking behind the platform. Worked extensively with Elena to finalize the screens so that they can get implemented. We used [Zeplin](https://zpl.io/11GBjj) to convert low fidelity mockups to high fidelity prototypes to make the process of converting design to working code easier:

![Prototypes Demo](/images/image10.png)  


**Progress**


Interests Screen:
![Interests_Screen](/images/image08.png)


Matchmaking Screen:
![Matchmaking_Screen](/images/image09.png)  

Commmits for this module: [Matchmaking Commits](https://github.com/P2Pvalue/teem/commits/staging?author=prastut)

## Mentors and Teem Project Members

I thank all of the following members for patiently listening to every doubt, motivating me when I was initially failing, and helping me become a more mature person:  

* [Samer Hassan](http://samer.hassan.name/), researcher at Berkman Klein Center, Harvard University. UCM Principal Investigator in the P2Pvalue. Leads research at Teem.
* [Antonio Tapiador](http://singularities.org/atd/), developer, sysops, researcher at UCM. 
* [Pablo Ojanguren](http://p2pvalue.eu/author/pojanguren/), lead developer of SwellRT, the Teem backend. 
* [Elena Martinez](http://p2pvalue.eu/author/emartinez/), lead designer at Teem. 

## If I could do it over? 
* Speed up design process by A/B testing: Since I wanted pixel perfection in my designs, I spent a lot of time on thinking about them, which I could have spent on evaluating my hypothesis in the open and then pivoting from there based on the insights from the data collected. 
* Hustle more myself: Initially I thought the role of mentors was to guide me at each and every step. I was stopping a lot of times, in the fear of doing something wrong. It would have surely reduced the counts of reiteration.    
* Get into the boots of the team: When you join another team, you start from scratch. Whereas the team members have been breathing in the project space for months! Asking more questions about the vision of the project and what it is trying to accomplish, leads your designs to be sensitive. Because a lighter shade of green doesn't matter in the large scheme of things.
* Work life balance: I would have spent some more time with my family and my friends, who were given a "not available" status in the GSoC period. Balance leads to less burnouts and sometimes change does wonder for that frustrating creative block.

## Conclusion

Participating in Google Summer of Code has brought me closer to the Teem's project members which comprises of an amazing group of developers and designers. The experience in working with them taught me essential soft skills like effective communication, designing for users and much more, than just writing code. I will be forever indebted to them for providing such a nurturing environment. 

Last but not the least, I thank Google for conducting Google Summer of Code. If it weren't for GSoC, I would have never learnt Python in depth, couldn't have been selected as a speaker at Pycon, wouldn't have the opportunity to present my work to Harvard "Geeks"!  

> "You can't connect the dots looking forward; you can only connect them looking backwards. So you have to trust that the dots will somehow connect in your future" - Steve Jobs.

I have been a fond believer of Google's vision through their products, and an avid follower of Google's design philosophy. Through GSoC, my belief in it's ethos becomes more stronger, of being a value driven company. I got to experience what it feels like to work in an organization which led me to gain more respect for Google because of the scale it operates on.