# XGari Garage Portal - Routing & Architecture Guide

This document outlines the essential frontend pages (UI routes) and their corresponding backend API endpoints required to build the Garage Portal. This structure is mapped directly to the Prisma database schema (specifically `GarageJob`, `Inspection`, and `ServiceItem`).

## 1. Dashboard (`/garage/dashboard`)
**Description:** The landing page for the garage owner/mechanic to see a quick overview of their daily work.

* **What they see:** 
  * Counters: "New Assigned Cars", "Cars In Progress", "Completed This Week".
  * A quick list of the top 3 urgent pending cars.
  * Total earnings/service charges pending from XGari Admin.

## 2. Assigned Jobs / Cars (`/garage/jobs`)
**Description:** A master list of all vehicles assigned to this specific garage by the Admin.

* **What they see:** A table or grid of assigned cars.
* **Filters:** Filter by `JobType` (Checkup vs. Service) and `JobStatus` (Pending, In Progress, Completed).
* **Backend API:** 
  * `GET /api/garage/jobs` - Returns all `GarageJob` records where `garageId` matches the logged-in user.

## 3. Job Workspace & Inspection (`/garage/jobs/:jobId`)
**Description:** The detailed view for a single car where the actual work happens. This is the most critical route for the garage workflow.

* **What they see:**
  * **Car Details:** Brand, Model, Year, Fuel Type, and Admin Notes (e.g., "Urgent checkup requested").
  * **Action Buttons:** "Start Job" (changes status from Pending -> In Progress), "Complete Job".
  * **Dynamic Forms (Based on Job Type):**
    * **If `CHECKUP` (Showroom Flow):** A checklist form to inspect the car (Engine, Suspension, Body) and select the `HealthCondition` (Excellent, Good, Risky).
    * **If `SERVICE` (Customer Repair):** A form to add `ServiceItem`s (e.g., "Replaced Brake Pads - Part cost: 2000, Labor: 500").
* **Backend APIs:** 
  * `PATCH /api/garage/jobs/:jobId/status` - To update job status (Start / Complete).
  * `POST /api/garage/jobs/:jobId/inspection-items` - To submit the health checklist for checkups.
  * `POST /api/garage/jobs/:jobId/service-items` - To add parts and labor costs for service requests.

## 4. Earnings & Invoices (`/garage/earnings`)
**Description:** A financial breakdown for the garage owner to track how much money they have made working with XGari.

* **What they see:** A history of completed jobs with a breakdown of `totalPartCost`, `totalServiceCharge`, and `grandTotal`.
* **Backend API:** 
  * `GET /api/garage/earnings` - Fetches completed `GarageJob`s for this garage and sums up their service charges.

## 5. Settings & Profile (`/garage/settings`)
**Description:** Basic profile management for the garage.

* **What they see:** Forms to update the garage name, contact phone number, address, and password.
* **Backend API:**
  * `PATCH /api/garage/profile` - Updates the User record associated with the garage.

---

### Suggested Workflow Example
1. Garage logs in, navigates to **`/garage/jobs`**, and sees a new `CHECKUP` job assigned by the Admin.
2. They click on the car, opening **`/garage/jobs/:jobId`**.
3. They click **"Start Job"** (This updates the DB and notifies the Admin that work has begun).
4. They perform the physical inspection, fill out the form on the page, and hit **"Submit Inspection"**.
5. They click **"Complete Job"**. 
6. *(Automatic)* The system immediately triggers the XGari Expert to review the garage's findings.
