# Falcons 
+ [GoVolunteer](#govolunteer)
+ [Payment](#payment)
+ [Camp](#camp)
+ [Trip](#trip)
+ [Childcare](#childcare)
+ [Lookup](#lookup)
+ [Waiver](#waiver)
+ [Ministry Platform Tools](#ministry-platform-tools)
+ [Event](#event)
+ [Other](#other)
+ [Summary](#summary)

## GoVolunteer
Can be broken as it is not in use and we are **planning** to replace it early next year and pull it entirely out of crds-angular. 

- Authorized() used in:
    - CincinnnatiRegistration (govolunteer/registration)
    - AnywhereRegistration (govolunteer/registration/{projectId})
    - GetGroupLeaderExportFile (go-volunteer/dashboard/export/{projectId})
- User token used in:
    - SaveRegistration
        - _goVolunteerService.CreateRegistration()
    - _goVolunteerService.CreateAnywhereRegistration()
    - Skills
        - _skillsService.RetrieveGoSkills()

## Payment
- Authorized() used in:
    - AlreadyPaidDeposit (invoice/{invoiceId}/has-payment)
    - GetPaymentDetails (invoice/{invoiceId}/payment/{paymentId})
    - PaymentConfirmation (payment/{paymentId/confirmation})
    - InvoicePaymentConfirmation (invoice/{invoiceId}/payment/{paymentId}/confirmation)
- User token used in:
    - PaymentService.DepositExists - used only to get ContactID
    - PaymentService.GetPaymentDetails - used to get ContactId and Email Address
    - PaymentService.SendPaymentConfirmation - used to get ContactId and Email Address
    - PaymentService.SendInvoicePaymentConfirmation - used to get ContactId and Email Address
## Camp
- Authorized() used in:
    - GetCampFamily (camps/{eventId}/family)
    - GetMyCampsInfo (camps/my-camp)
    - GetCampEventDetails (camps/{eventId})
    - GetCampProductDetails (camps/{eventId}/product/{contactid})
    - GetCamperInfo (camps/{eventId}/campers/{contactId})
    - SaveProductDetails (camps/product)
    - SaveCampReservation (camps/{eventId}/campers)
    - GetCampWaivers (camps/{eventId}/waivers/{contactId})
    - GetCampMedicalInfo (camps/{eventId}/medical/{contactId})
    - SaveMedicalInformation (camps/medical/{contactId})
    - SaveWaivers (camps/{eventId}/waivers/{contactId})
    - SaveCamperEmergencyContact (camps/{eventId}/emergencycontact/{contactId})
    - CamperConfirmation (camps/{eventId}/confirmation/{contactId})
    - GetCamperEmergencyContact (camps/{eventId}/emergencycontact/{contactId})
- User token used in:
    - CampService.GetEligibleFamilyMembers - Used to get ContactId and HouseholdId
    - CampService.GetMyCampInfo - Used only to get HouseholdId
    - CampService.GetCampProductDetails - Used to call PaymentService.GetPaymentDetails
    - CampService.GetCamperInfo - Used only to get HouseholdId
    - CampService.SaveInvoice - Used to get ContactId and HouseholdId
    - CampService.SaveCampReservation - Used only to get HouseholdId
    - CampService.GetCampMedicalInfo - Used only to get HouseholdId
    - CampService.SaveCamperMedicalInfo - Used only to get HouseholdId
    - CampService.SaveWaivers - Used only to get ContactId
    - CampService.SaveCamperEmergencyContactInfo - Used to get HouseholdId and to call GetCamperEmergencyContactInfo, which takes a token and does nothing with it.
    - CampService.SendCampConfirmationEmail - takes a token for no reason
    - CampService.GetCamperEmergencyContactInfo - takes a token for no reason
## Trip
- Authorized() used in:
    - GetFamilyWithTripInfo (trip/{campaignId}/family-members)
    - ContactHasScholarship (trip/scholarship/{campaignId}/{contactId})
    - TripFormResponses (trip/form-responses/{selectionId}/{selectionCount}/{recordId})
    - GetCampaigns (trip/campaign/{campaignId})
    - GeneratePrivateInvite (trip/generate-private-invite)
    - MyTrips (trip/mytrips)
    - ValidatePrivateInvite (trip/validate-private-invite/{campaignId}/{invitationKey})
    - GetLoggedInContact (trip/user)
    - GetIPromiseDocument (trip/ipromise/{tripEventParticipantId})
    - ReceiveIPromiseDocument (trip/ipromise)
- User token used in:
    - TripService.GetFamilyMembers - Calls ServeService.GetImmediateFamilyParticipants which uses it to get ContactId and to call ContactRelationshipRepository and uses the token in a db call. *******
    - TripService.GeneratePrivateInvite - Calls PrivateInviteService.Create which uses tokens in db calls *******
    - TripService.GetMyTrips - Calls ServeService.GetImmediateFamilyParticipants which uses it to get ContactId and to call ContactRelationshipRepository and uses the token in a db call. ******* 
    - TripService.ValidatePrivateInvite - Used to get email address
    - TripController.GetLoggedInContact - Used to get logged in contact, duh. 
## Childcare
- Authorized() used in:
    - SaveRsvp (childcare/rsvp)
    - ChildcareEventById (childcare/event/{eventId})
    - ChildrenEligibleForChildcare (childcare/eligible-children)
    - CreateChildcareRequest (childcare/request)
    - UpdateChildcareRequest (childcare/updaterequest)
    - ApproveChildcareRequest (childcare/request/approve/{requestId})
    - RejectChildcareRequest (childcare/request/reject/{requestId})
    - GetChildcareRequest (childcare/getrequest/{requestId})
    - ChildcareDashboard (childcare/dashboard/{contactId})
- User token used in:
    - EventService.GetMyChildcareEvent - Used to call ParticipantService.GetParticipantRecord which uses token in db call. *******
    - ChildcareService.MyChildren - Used to call ServeService.GetImmediateFamilyParticipants which uses it to get ContactId and to call ContactRelationshipRepository and uses the token in a db call. *******
    - ChildcareService.CreateChildcareRequest - Uses token in ChildcareRequestRepository to make db call AND in ChildcareRequestRepository.GetChildcareRequest to make a db call *******
    - ChildcareService.UpdateChildcareRequest - Uses token in ChildcareRequestRepository to make db call AND in ChildcareRequestRepository.GetChildcareRequest to make a db call *******
    - ChildcareService.ApproveChildcareRequest
        - ChildcareService.GetChildcareRequestForReview - Takes token for no reason. 
        - EventRepository.CreateEventGroup - Uses token for db call, but will default to API token if not provided.
        - ChildcareService.SendChildcareRequestDecisionNotification - Uses token in ChildcareRequestRepository.GetChildcareRequest to make a db call *******
    - ChildcareService.RejectChildcareRequest - Calls Childcare.Service.SendChildcareRequestDecisionNotification, see above.
    - ChildcareController.ChildcareDashboard - Uses token to get Person object, in which it uses ContactId and HouseholdId
## Lookup
- Authorized() used in:
    - Lookup (lookup/{table?})
    - FindGroups (lookup/group/{congregationId}/{ministryId})
    - FindChildcareTimes (lookup/childcare-times/{congregationId})
    - EmailExists (lookup/{userId}/find/{email?})
- User token used in: 
    - All the lookups, but its optional and if omitted, the api token is used.
    - LookupRepository 
        - GroupsByCongregationAndMinistry()
        - ChildcareTimesByCongregation()
        - EmailSearch()
## Waiver
- Authorized() used in:
    - GetEventWaivers (waivers/event/{eventId})
    - GetWaiver (waivers/{waiverId})
    - SendAcceptWaiverEmail (waivers/{waiverId}/send/{eventPartipantId})
- User token used in:
    - GetEventWaivers - Used only to get contactId
    - SendAcceptWaiverEmail - Used only to get contactId
## MinistryPlatformTools
- Authorized() used in:
    - GetPageSelectionRecordIds (mptools/selection/{selectionId})
- User token used in:
    - _selectionService.GetSelectionRecordIds - Unclear if user token is needed or if api token can be used. *******
## Event
Much of this is for the Create/Edit Event Tool which Falcons don't own -Unless you ask Angie
- Authorized() used in:
    - RsvpToEvent (event)
    - EventById (event/{eventId})
    - CopyEventSetup (event/copyeventsetup)
    - GetEventsBySite (event/eventsbysite/{site})
    - GetEventTemplatesBySite
- User token used in:
    - RsvpToEvent -used in EventService.SendRsvpMessage to get contactId and email address
    - CopyEventSetup -EventService.CopyEventSetup
        - Heavily used, unsure if user token is needed or if api token can be used. *******
    - GetEventsBySite -Used to get events, should be fine with API token
    - GetEventTemplatesBySite -Same as above. Possibly shared code fixed in one place?
## Other
- On-behalf-of functionality currently uses the logged in user's token, but can be made to work with UserID, since that's what the API actually needs. 

# Summary
|Reason for token|# of updates|Effort|
|---|:---:|:---:|
|ContactID| ~12 | S |
|HouseholdID| ~9 | S |
|Email Address| ~3  | XS |
|Database Call| ~10 | XL-XXL |
|Misc (default can be used)| ~8 | --- |
|**Total** | ~43 | XL |