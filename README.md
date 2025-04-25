# TIM Contracts - Firebase Studio App

This repository contains the Firebase-native implementation plan for refactoring the Contract Preparation module of the Timber Information Manager (TIM) application using Firebase Studio and associated tools.

## 📐 Architecture Overview
This app is built around Firebase services:
- Firebase Studio (UI)
- Firestore (data)
- Firebase Auth (roles)
- Firebase Functions (workflow logic)
- Cloud Storage (file uploads)
- Firebase Extensions (PDF generation)
- Firebase Hosting (deployment)

## 🔁 Gates Covered
- **Gate 1**: Plan (volume estimates, certification)
- **Gate 2**: Design (NEPA decisions)
- **Gate 3**: Prepare (cruise data)
- **Gate 4**: Advertise (appraisal, bids)
- **Gate 5**: Bid (recording and reporting)
- **Gate 6**: Award (final contracts, bills)

## 🔐 Role-Based Access
Roles like `contract_preparer`, `contract_agent`, and `data_admin` are enforced using Firebase Auth custom claims and Firestore rules.

## 📦 Files Included
- `firebase_schema_gate1_to_gate6.json`: full Firestore schema
- `firebase_studio_tim_plan.md`: detailed implementation blueprint

## ✅ Status
This repo serves as the foundation for Firebase Studio import and app generation.

## 🚀 Next Step
Import this repo into [Firebase Studio](https://firebase.google.com/studio) to begin app creation.

---
