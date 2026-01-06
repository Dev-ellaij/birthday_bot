Birthday Bot 

 Overview

The Birthday Bot is an automated celebration system built for the Book Club. It:

* Stores members and their birthdays
* Displays birthdays in a **Flutter + Gemini UI** (calendar + carousel)
* Automatically sends **WhatsApp birthday messages**
* Automatically sends **birthday emails**
* Runs fully automatically using **Serverpod scheduled tasks**

This project is optimized for **hackathons**, but is architected cleanly enough to scale later.

---

## High-Level Architecture

```
Flutter + Gemini (UI)
        │
        ▼
Serverpod Backend (Dart)
        │
        ├── PostgreSQL (Members & Birthdays)
        ├── Scheduler (Daily cron)
        │       ├── WhatsApp (CallMeBot / Venom)
        │       └── Email (SMTP)
        ▼
Members receive WhatsApp + Email
Tech Stack

 Frontend

* Flutter
* Gemini UI components
* table_calendar
* carousel_slider
* Dio (HTTP client)

### Backend

* Serverpod (Dart)
* PostgreSQL
* Scheduled Tasks (cron-like)
* HTTP package
* mailer (SMTP emails)

### Messaging

* WhatsApp via CallMeBot (non-official, hackathon-safe)
* Email via Gmail SMTP (or any SMTP provider)

---

## Data Model

### Member Table

| Field       | Type     | Description       |
| ----------- | -------- | ----------------- |
| id          | int      | Primary key       |
| name        | String   | Member name       |
| birthday    | DateTime | Date of birth     |
| phoneNumber | String   | WhatsApp number   |
| email       | String   | Email address     |
| photoUrl    | String   | Profile image URL |

---

## Backend – Serverpod

### Endpoints

#### 1. Add Member

Stores a new member in the database.

**Method:** `addMember()`

**Used by:** Flutter app

---

#### 2. Get All Birthdays

Returns all members and their birthdays.

**Method:** `getAllBirthdays()`

**Used by:** Calendar view

---

#### 3. Get Today’s Birthdays

Returns only members whose birthday is today.

**Method:** `getTodaysBirthdays()`

**Used by:** Carousel + scheduler

---

## Scheduler (Core Automation)

### What it does

Runs **once per day (e.g., 9AM)** and:

1. Fetches today’s birthdays
2. Sends WhatsApp messages
3. Sends birthday emails
4. Logs activity

### Why Scheduler?

* Fully automatic
* No manual trigger needed
* Reliable and scalable

---

## WhatsApp Integration

### Option Used: CallMeBot

**Why CallMeBot?**

* No Meta approval
* No Twilio
* Works immediately
* Perfect for small communities & hackathons

### Message Flow

```
Serverpod Scheduler
   ↓
HTTP GET Request
   ↓
CallMeBot API
   ↓
WhatsApp message delivered
```

### Message Format Example

> 🎉 Happy Birthday, Blessing! 🎂
>
> From all of us at the Book Club 💖

### Safety Notes

* Only send to opted-in members
* Low volume (1 message/day)
* Hackathon-safe

---

## Email Integration

### Method

* SMTP via `mailer` package
* Gmail App Password recommended

### Email Content

**Subject:** Happy Birthday 🎉

**Body:**

```
Hi Blessing,

🎂 Happy Birthday from all of us at the Book Club!
We’re so glad to celebrate you today.

With love,
Book Club Team
```

---

## Frontend – Flutter + Gemini

### Main Screens

#### 1. Home Screen

* Calendar view
* Highlights days with birthdays

#### 2. Carousel Screen

* Opens when a day is clicked
* Displays birthday members
* Auto-scrolls

#### 3. Birthday Card

* Member photo
* Name
* Birthday greeting

---

## Calendar Interaction Flow

```
User opens app
   ↓
Calendar shows birthday markers
   ↓
User taps a day
   ↓
Carousel opens
   ↓
Birthday cards displayed
```

---

## UX Decisions (Why Judges Will Like It)

* Visual celebration (carousel)
* Human-centered (birthdays)
* Automation (scheduler)
* Real-world use
* Clean architecture

---

## Deployment Strategy

### Backend

* Local for demo
* Docker / VPS for production

### Frontend

* Android / iOS
* Web (optional)

---

## Security & Privacy

* No sensitive data exposed
* Phone numbers stored server-side
* No hardcoded secrets in frontend
* SMTP credentials kept private

---

## Future Enhancements

* Switch to official WhatsApp Cloud API
* Add push notifications
* Add admin dashboard
* Add celebration analytics
* Group birthday messages

---
ove for the Book Club ✨
