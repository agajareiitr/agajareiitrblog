---
title: "Covid19 India Whatsapp Application"
tags: ['Python', 'Twilio', 'ngrok' ,'Flask']
author: "Akash Gajare"
showToc: true
TocOpen: false
draft: false
hidemeta: false
comments: false
description: "Covid19 India Whatsapp Application"
canonicalURL: "https://agajareiitr.github.io/projects/covid19-india-whatsapp-application/"
disableHLJS: true # to disable highlightjs
disableShare: false
disableHLJS: false
hideSummary: false
searchHidden: false
ShowReadingTime: true
ShowBreadCrumbs: true
ShowPostNavLinks: true
cover:
    image: "covid19-india-whatsapp-application.png" # image path/url
    alt: "covid19-india-whatsapp-application.jpg" # alt text
    caption: "Source : Google Images" # display caption under cover
    relative: false # when using page bundles set this to true
    hidden: false # only hide on current single page
---

<p align="center">
  <a href="https://github.com/agajareiitr/COVID-19india-WhatsApp-Application"> 
  <img  src="https://github-readme-stats.vercel.app/api/pin/?username=agajareiitr&repo=COVID-19india-WhatsApp-Application&bg_color=151515&text_color=9f9f9f"/>
    </a>
</p>

# COVID-19india WhatsApp Application

- COVID-19india-WhatsApp is a Application Which Provides Latest Information of Corona Virus Cases on Your WhatsApp Number

- [WhatsApp Bot Link](https://wa.me/14155238886?text=join%20second-previous) ( `sorry for inconvience but the bot is currently inactive` )

---

## Setup

**STEP 1 :** Send **`join second-previous`** to WhatsApp Number - **`+14155238886`**

<img src="/Project-Images/join%20second-previous.PNG" height=450>

**STEP 2 :** A simple **`Hey`** Message will Guide you further and you will get a **Welcome message** like below screenshot:

**STEP 3 :** You can choose Any option like :

- ` 1` for **Latest Corona Virus Cases of All India**
- ` 2` for **To get state code and then Enter the State Code to get State Cases**
- ` 3` for **5 most Affected States in India**
- ` 4` for **Latest News on Corona virus cases**
- ` 5` for **How I Made this bot**
- ` 6` for **Share this Bot with a Link**
- or Enter the Name of State like `Maharashtra` to get most affected districts in **Maharastra**

---

## Source of COVID-19 Cases Data

- COVID-19india WhatsApp Application uses the **OPEN SOURCED API** Provided by https://github.com/covid19india to Get the latest data on Corona Virus Cases Data.

- You can Always Cross check Data on https://www.covid19india.org/

- This Website is created by a group of dedicated volunteers who curate and verify the data coming from several sources on Corona Cases and Provide the Lastest Updates and They own all the Data Which this Application provide on Your whatsApp

---

## How This Works

COVID-19india WhatsApp Application uses following things to operate:

- Uses **` Python `** **` Twilio `** **` ngrok `** **` Flask `**

- **Step 1 :** Using Python it collected The data from the from API of **covid19india** using **`requests`** Module
- **Step 2 :** Using **`Flask` and `Twilio`** Python Libraries I hosted it on localhost and arranged **'POST','GET'** Methods
- **Step 3 :** using **`ngrok`** to convert my localhost into **`'http' server`**
- **Step 4 :** Added **`'http' server into Twilio`** for sending WhatsApp Messages

---
