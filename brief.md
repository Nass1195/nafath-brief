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

## Cybersecurity

## Data

## Misconception in My Track

## Open Question
