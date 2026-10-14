Version: NAFATH-V2
Track: AI

## Overview

A telecom operator is launching a 5G service for emergency responders, and it must keep working during congestion, outages and cyber attacks. The six fields support each other in this goal. Networking provides the base: a dedicated 5G network slice reserves capacity for responders. Software keeps requests reliable, because the app's server calls must be safe to retry after a failure. Mobile keeps responders productive when the signal drops, by saving data offline and syncing it later. AI predicts congestion so the operator can act before the network is overloaded. Cybersecurity prepares the team to detect, contain and recover from attacks. Data turns usage records into daily regional reports that show whether the service is meeting its goals. Together, these fields make the service dependable when it matters most.

## Software

Safe methods (GET, HEAD, OPTIONS, TRACE) only read data and do not change anything on the server. Idempotent methods give the same result whether sent once or many times; these are the safe methods plus PUT and DELETE. POST and PATCH are neither. This matters for automatic retries: if a responder's app retries a failed POST, it could create a duplicate incident report. Clients should only retry idempotent requests automatically, or use a unique request ID.

Resource: RFC 9110 - HTTP Semantics - https://www.rfc-editor.org/info/rfc9110/

## AI

Overfitting happens when a model learns the training data too closely, so it scores well on that data but poorly on new data. Cross-validation detects this by testing on data the model did not train on. In k-fold cross-validation, the data is split into k parts, and each part is used once as the test set. A large gap between training and validation scores shows overfitting. For time-ordered congestion data, a time-series split avoids training on the future.

Resource: Cross-validation: evaluating estimator performance - scikit-learn - https://scikit-learn.org/stable/modules/cross_validation.html

## Mobile

The app should be offline-first: it saves data on the device and lets responders keep working without a signal. New actions, such as status updates, go into a local queue. When the connection returns, the app syncs in the background, sending queued changes and downloading new data. If the same record changed in two places, a conflict rule such as last-write-wins decides the result. The app should clearly show what is still waiting to sync.

Resource: Build an offline-first app - Android Developers - https://developer.android.com/topic/architecture/data-layer/offline-first

## Networking

Network slicing runs several logical networks on the same physical 5G infrastructure. Each slice is isolated and secure, and can be configured for its own needs, such as low latency, high throughput or high reliability. For this service, the operator can create a public safety slice that gives responders prioritized, mission-critical communications. During a crowded event or emergency, the slice scales with demand without interfering with other slices.

Resource: Network slicing - Ericsson - https://www.ericsson.com/en/network-slicing

## Cybersecurity

NIST's incident response lifecycle has four phases that run as a loop. Preparation: write the plan, assign roles, set up monitoring and test backups. Detection and Analysis: notice unusual activity, such as a traffic spike, then confirm the incident and its scope. Containment, Eradication and Recovery: isolate affected systems, remove the threat and restore service. Post-Incident Activity: review lessons learned and feed them back into preparation. In 2025, NIST reorganized them around the CSF 2.0 functions.

Resource: The NIST Incident Response Lifecycle: 4 Phases Explained - Consilien - https://consilien.com/news/nist-incident-response-lifecycle-4-phases-explained

## Data

A star schema has one central fact table, such as daily usage records, linked directly to denormalized dimension tables like Region and Date. A snowflake schema normalizes dimensions into extra tables, for example City, Region and Country. Snowflaking reduces repeated data but adds joins and complexity. For reports of usage per region per day, a star schema is the better fit: queries filter by date and region, then summarize usage, with fewer joins.

Resource: Understand star schema and the importance for Power BI - Microsoft Learn - https://learn.microsoft.com/en-us/power-bi/guidance/star-schema

## Misconception in My Track

Misconception: many people believe cross-validation prevents overfitting, so a model that passed cross-validation cannot be overfitted. Correct version: cross-validation does not change how the model learns. It only measures how well the model is likely to perform on data it has not seen, which makes overfitting visible as a gap between training and validation scores. For time-ordered congestion data, shuffled folds can even hide overfitting by giving an inflated score. Proof: scikit-learn, section 3.1.5 "Permutation test score": "Cross-validation provides information about how well an estimator generalizes by estimating the range of its expected scores."

## Open Question

I would verify whether TimeSeriesSplit gives a realistic score for the congestion model, by testing it on data from a real past emergency, such as a large event or outage, which may look very different from normal days in the training data.