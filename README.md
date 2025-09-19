# On-call Simulation LLM Feedback

This repo contains the code for an experimental feedback engine built into a medical on-call simulation, developed by **Dr Tom Brisk**. 

The original project ("Awaiting Results": Simulating Real-World Delays in an On-Call Simulation) forms the shoulders upon which this new experimental project stands. 

---

## Background

The simulation drops final-year medical students into a mock out-of-hours / on-call shift as a newly-qualified doctor (FY1). Over two hours, they are “bleeped” around the hospital to review simulated patients and are drip-fed information to decide on a management plan.  

The idea is to bridge the gap between:
- being a medical student who can answer textbook exam questions, and  
- being a doctor who juggles multiple complex patients, time pressures, and uncertainty.  

During the session students can:
- request investigations (via the [**Investigation Request Portal**](https://docs.google.com/forms/d/e/1FAIpQLSfIO_j8C_hzjbyOpY0k93aVdbJfCUy8bfmcywoEf7eghziUoA/viewform?usp=dialog) (a Google Form with an Apps Script running in the background),  
- escalate to senior clinicians (played by facilitators),  
- prescribe medications, and  
- most importantly, **document their clinical impression and plan**.  

The Investigation Request Portal is what made the original project different to other simulation sessions. Students rarely experience managing clinical ambiguity without test results to guide them. Unfortunately, clinicians often must take appropriate steps before any results are back. 

With the Portal, students have to wait for any results to come through. Want an ECG? That will take 2 minutes to be emailed to you. Blood tests? They will take 30. A CT scan? I'm afraid you'll have to wait an hour. 

*Do I start antibiotics before the inflammatory markers are back? Do I speak to the orthopaedic doctor before or after I have an X-ray of the suspected hip fracture? Should I start the patient with a suspected clot in the lungs on blood thinners before I have a scan result, or do I need to confirm it on a CT scan first?* These are the sorts of questions that are better addressed in a simulated environment than at 2am on your first ever night shift. 

Anyway, that's the original project which I'll soon be presenting at a conference, so I'm deliberately keeping the cards close to my chest. 

**The student's documentation is where this project comes in.**

---

## Problem identified

1. Feedback quality on simulated clinical documentation is highly facilitator-dependent.  
2. Facilitators consistently run out of time to cover everything in the debrief (a 2-hour sim sessions needs a loooooong debrief!).  

---

## Proposed solution

Use **LLM-generated feedback** for students’ written documentation.  
- Each time a student submits a plan (via Google Form -> Sheet), an LLM (Gemini in this case) reviews it against a rubric and case context.  
- Students then receive timely, structured, and consistent feedback by email.  

This frees up valuable debrief time to focus on bigger questions about on-call practice, escalation, and workload management.


---

## How it works

- **Google Form** -> students document their Clinical Impression & Plan.  
- **Google Sheet** (tab: `documentationResponses`) collects responses.  
- **Google Apps Script** (this repo, synced with clasp) runs on submission.  
- **Rubric tab** defines general criteria (reasoning, prioritisation, escalation, safety-netting, etc.) and feedback stems.  
- **Case context tab** defines patient-specific details (obs, red flags, exemplar management plan).  
- **Gemini API** is called with both -> returns JSON feedback.  
- Feedback is written back into the Sheet and emailed to the student automatically.  

---

## Limitations 

This project is very much in a trial phase. A few major limitations have already been identified as areas to work on: 
- The LLM sometimes hallucinates. In particular, it will sometimes praise students for content not actually written by them. This has been partially mitigated by prompt rules. 
- The LLM may criticise students for missing a differential diagnosis even if they have correctly requested investigations that would facilitate a diagnosis. E.g. "You missed the diagnosis of acute kidney injury" despite the student ordering appropriate blood tests to assess kidney function. This will be addressed with the addition of explicit and implicit logic to prompting.

--- 

## Summary 

GenAI is obviously going to transform elements of medical education. This project has some significant limitations, but shows a glimpse into the benefts that a simple LLM API can offer.
