1. Dashboard (/garage/dashboard)
Description: The landing page for the garage owner/mechanic to see a quick overview of their daily work.

What they see:
Counters: "New Assigned Cars", "Cars In Progress", "Completed This Week".
A quick list of the top 3 urgent pending cars.
Total earnings/service charges pending from XGari Admin.
2. Assigned Jobs / Cars (/garage/jobs)
Description: A master list of all vehicles assigned to this specific garage by the Admin.

What they see: A table or grid of cars.
Filters: Filter by JobType (Checkup vs. Service) and JobStatus (Pending, In Progress, Completed).
Backend API Idea: GET /api/garage/jobs (Returns all GarageJob records where garageId matches the logged-in user).
3. Job Workspace & Inspection (/garage/jobs/:jobId)
Description: The detailed view for a single car where the actual work happens. This is the most important route for the garage.

What they see:
Car Details: Brand, Model, Year, Fuel Type, and Admin Notes (e.g., "Urgent checkup requested").
Action Buttons: "Start Job" (changes status from Pending -> In Progress), "Complete Job".
Dynamic Forms (Based on Job Type):
If CHECKUP (Showroom Flow): They get a checklist form to inspect the car (Engine, Suspension, Body) and select the HealthCondition (Excellent, Good, Risky).
If SERVICE (Customer Repair): They get a form to add ServiceItems (e.g., "Replaced Brake Pads - Part cost: 2000, Labor: 500").
Backend API Ideas:
PATCH /api/garage/jobs/:jobId/status (To start or complete the job).
POST /api/garage/jobs/:jobId/inspection-items (To submit the health checklist).
POST /api/garage/jobs/:jobId/service-items (To add parts and labor costs).
4. Earnings & Invoices (/garage/earnings)
Description: A financial breakdown for the garage owner to track how much money they have made working with XGari.

What they see: A history of completed jobs with a breakdown of totalPartCost, totalServiceCharge, and grandTotal.
Backend API Idea: GET /api/garage/earnings (Fetches completed GarageJobs and sums up their service charges).
5. Settings & Profile (/garage/settings)
Description: Basic profile management.

What they see: Ability to update garage name, contact phone number, address, and password.
Suggested Workflow for the Garage UI:
Garage logs in, goes to /garage/jobs and sees a new CHECKUP job assigned.
They click on the car, opening /garage/jobs/123.
They click "Start Job" (This notifies the Admin that work has begun).
They fill out the 10-point inspection form on the page and hit "Submit Inspection".
They click "Complete Job".
(Automatic) The system immediately triggers the XGari Expert to review the garage's findings.
