# On-call Simulation LLM Feedback

This repo contains the code for an experimental feedback engine built into a medical on-call simulation, developed by **Dr Tom Brisk**.

---

## Background

The simulation drops final-year medical students into a **mock out-of-hours shift** as a newly-qualified doctor. Over two hours, they are “bleeped” around the hospital to review simulated patients and are drip-fed information to decide on a management plan.  

The idea is to bridge the gap between:
- being a medical student who can answer textbook exam questions, and  
- being a doctor who juggles multiple complex patients, time pressures, and uncertainty.  

During the session students can:
- request investigations (via the **Investigation Request Portal**),  
- escalate to senior clinicians (played by facilitators),  
- prescribe medications, and  
- most importantly, **document their clinical impression and plan**.  

This documentation is where this project comes in.

---

## Problem identified

1. Feedback quality on documentation is **highly facilitator-dependent**.  
2. Facilitators consistently run out of time to cover everything in the debrief.  

---

## Proposed solution

Use **LLM-generated feedback** for students’ written documentation.  
- Each time a student submits a plan (via Google Form → Sheet), an LLM reviews it against a rubric and case context.  
- Students then receive **timely, structured, and consistent feedback** by email.  

This frees up valuable debrief time to focus on **bigger questions** about on-call practice, escalation, and workload management.


---

## How it works

- **Google Form** → students document their Clinical Impression & Plan.  
- **Google Sheet** (tab: `documentationResponses`) collects responses.  
- **Google Apps Script** (this repo, synced with clasp) runs on submission.  
- **Rubric tab** defines general criteria (reasoning, prioritisation, escalation, safety-netting, etc.) and feedback stems.  
- **Case context tab** defines patient-specific details (obs, red flags, exemplar management).  
- **Gemini API** is called with both → returns JSON feedback.  
- Feedback is written back into the Sheet and emailed to the student automatically.  

---

## Limitations 

This project is very much in a trial phase. A few major limitations have already been identified as areas to work on: 
- The LLM sometimes hallucinates. In particular, it will sometimes praise students for content not actually written by them. This has been partially mitigated by prompt rules. 
- The LLM may criticise students for missing a differential diagnosis even if they have correctly requested investigations that would facilitate a diagnosis. E.g. "You missed the diagnosis of acute kidney injury" despite the student ordering appropriate blood tests to assess kidney function. This will be addressed with the addition of explicit and implicit logic to prompting.
