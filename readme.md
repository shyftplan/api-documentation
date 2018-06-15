# shyftplan API documentation

All available REST API urls and parameters are listed and described at <https://shyftplan.com/swagger>.

Every API call needs authentication with the params `user_email` and `authentication_token`.
The email is the one used for signing in into shyftplan. It is recommended to create a separate Login for using the API.

The authentication token can be acquired by two ways:

- create an authentication token for your current employment on your profile
- See <https://shyftplan.com/swagger#!/login> to create new login via API

  [POST] <https://shyftplan.com/api/v1/login?user[email]=email&user[password]=password>

  where `email` and `password` are the credentials to log into shyftplan

  The response contains the authentication token:

  ```js
  {
    "success": true,
    "info": "Logged in",
    "authentication_token": ":token:",
    ...
  }
  ```

## List Actions

Any read `list action` is paginated with the params `page` (page number)  and `per_page` (amount of records per page). The default page size is `1000` for most of the actions.

This is how the response of most list actions looks like:

```js
{
  "items": [
    ...
  ],
  "total": 1337
}
```

Some list actions allow an Array of parameters (for example `ids` at [Shifts](https://shyftplan.com/swagger#!/shifts/getApiV1Shifts)).
That parameters can't be passed by the swagger UI. If you want to use them you need to pass them like `?ids[]=1&ids[]=2&ids[]=3` ??? @Tamino in the URL?

## Absence

`Users` are able to inform their employer about their `absences` or stakeholders create them for them. It contains the `absence reason`, time of absence and whether it is paid or not. Optionally a note for further explanation can be added.  The employer can then approve or decline the absence.

- See <https://shyftplan.com/swagger#!/absences/getApiV1Absences> for fetching all absences.
- See <https://shyftplan.com/swagger#!/absences/postApiV1Absences> to create an absence.
- See <https://shyftplan.com/swagger#!/absences/getApiV1AbsencesEmployments> for fetching all employments and their work hours information.
- See <https://shyftplan.com/swagger#!/absences/getApiV1AbsencesEmploymentsId> for fetching an employment and its work hours information.
- See <https://shyftplan.com/swagger#!/absences/deleteApiV1AbsencesId> to delete an absence.
- See <https://shyftplan.com/swagger#!/absences/getApiV1AbsencesId> for fetching an absence.
- See <https://shyftplan.com/swagger#!/absences/putApiV1AbsencesId> to update an absence.
- See <https://shyftplan.com/swagger#!/absences/postApiV1AbsencesIdApprove> to approve an absence.
- See <https://shyftplan.com/swagger#!/absences/postApiV1AbsencesIdDecline> to decline an absence.


Example `bsence` json:

```js
{
  "id": 1337,               // Id of Absence
  "starts_at": "",		
  "ends_at": "",            // Timestamps
  "state": "accepted",      // State of Absence (new/accepted/declined)
  "reason": "other",		// type of Absence Reason (vacation/sickness/overtime/other)
  "notes": "",              // Note added by Employee or Employer
  "employment_id": 7331,	//Id of Employment
  "created_at": "",
  "updated_at": "",		
  "deleted_at": "",         // Timestamps
  "days": 0,                // Work days
  "vacation_minutes": 0,    // Amount of Vacation Minutes
  "paid": true,             // Will the employee be paid during the absence?
  "absence_reason_id": 3713, //Id of Absence Reason
  "is_full_day": true,      // An Absence can vary between Full Day(s) or set by Time of Day
  "can_manage": true		// This is depending on given Stakeholder Rights
}
```

## Absence Reason

In order do distinguish between different kind of `absences` every `absence` has an `absence reason`.
Note that `vacation` as an `absence reason` will automatically count off vacation days that have been documented in an employee's payroll information.

- See <https://shyftplan.com/swagger#!/absence_reasons/getApiV1AbsenceReasons> for fetching all absence reasons.


Example `Absence` `Reason` json:

```js
{
  "id": 110301,         // Id of Absence Reason
  "company_id": 1337,   // Id of Company
  "name": "vacation",   // Preset Absence Reason (vacation, sickness, overtime, other)
  "has_localization": true, // ??? @tamino
  "deleted_at": "",
  "created_at": "",
  "updated_at": "",     // Timestamps
},
```

## Availability

Every `employee` can inform stakeholders about their `availabilities`. The `employee` is able to state whether he or she is available or unavailable, either all day or within a certain time frame and if they should be weekly repeated or appear only once.

- See <https://shyftplan.com/swagger#!/availabilities/getApiV1Availabilities> for fetching all availabilities.
- See <https://shyftplan.com/swagger#!/availabilities/postApiV1Availabilities> to create a new availability.
- See <https://shyftplan.com/swagger#!/availabilities/getApiV1AvailabilitiesAvailabilityId> for fetching an existing availability.
- See <https://shyftplan.com/swagger#!/availabilities/getApiV1AvailabilitiesAvailabilityIdExceptions> for fetching all recurring availabilities.
- See <https://shyftplan.com/swagger#!/availabilities/deleteApiV1AvailabilitiesAvailabilityId> to (PERMANENTLY) delete an existing availability.
- See <https://shyftplan.com/swagger#!/availabilities/putApiV1AvailabilitiesAvailabilityId> for editing existing availability.

??? @tamino what is an availability exception

- See <https://shyftplan.com/swagger#!/availabilities/getApiV1AvailabilitiesExceptions> for fetching all availability exceptions.
- See <https://shyftplan.com/swagger#!/availabilities/getApiV1AvailabilitiesExceptionsExceptionId> for fetching an existing availability exception.
- See <https://shyftplan.com/swagger#!/availabilities/postApiV1AvailabilitiesAvailabilityIdExceptions> to create a new availability exception.
- See <https://shyftplan.com/swagger#!/availabilities/deleteApiV1AvailabilitiesExceptionsExceptionId> to (PERMANENTLY) delete an existing availability exception.

Example `Availability` json:

```js
{
  "id": 1337,             // Id of the availability
  "employment_id": 4771,  // Id of the employment
  "starts_at": "",
  "ends_at": "",          // Timestamps
  "repeat": false,        // Is the availability repeated weekly?
  "available": true,      // Available is true, unavailable is false
  "created_at": "",
  "updated_at": ""        // Timestamps
}
```

## Company

Every `user` is associated with at least one `company`. It contains important settings about the use of the shyftplan software.

- See <https://shyftplan.com/swagger#!/companies/getApiV1Companies> for fetching all companies.
- See <https://shyftplan.com/swagger#!/companies/getApiV1CompaniesId> for fetching an existing company.

Example `Company` json:

```js
{
  "id": 1337,                     // Id of Company
  "name": "",                     // Name of Company
  "description": "",              // Description of Company
  "created_at": "",
  "updated_at": "",               // Timestamps
  "image_uid": "",                // Default Image are the Company Initials
  "image_name": "",               // Name of Initials Image
  "view_personal": false,         // Allow Employees to see other Employees in their Positions
  "round_to": 5,                  // Punch Time Setting ‚Round to‘ 0, 5, or 15
  "round_fair": false,            // Round for employee’s benefit?
  "employee_shift": true,         // Can an employee create a shift by starting the punch time?
  "auto_punchout": true,          // End punch time automatically?
  "punchout_hour": 1,             // After how many hours should the punch time be ended automatically?
  "edit_shift_payments": false,   // Users can submit a different shift payment
  "onboarding_step": 5,           // Company Onboarding Tutorial has 5 steps that need to be completed before using the shyftplan software
  "billing_method": "Direct",     // Billing Method either direct, corrected or not set
  "special_date_counted_off": false,  // Has the employee off from work if only one of several locations has a special day
  "probable_employees": 1,        // Number of Employees stated during Registration
  "auto_accept_shift_requests": null, // When enabled Shifts can be auto accepted
  "survey_done_at": "",           // Timestamp
  "picture_data": {
          ...
      }
  "currency": "€",                // Type of Currency
  "time_zone": "Europe/Berlin",   // Setting of preferred Time Zone
  "show_user_token": true,        // Show Lunchtime Token on User Modal
  "round_to_shift": true,         // Round to Shift Start
  "invoice_email": "",            // Company Email Sddress
  "assignment_group_enabled": false,  // Show Assignment Groups
  "is_deactivated": false,            // Is Billing Method deactivated?
  "is_overassignment_allowed": false, // Are Overassignments allowed?
  "is_dashboard_messages_allowed": false,   // Enable Employees to send Messages on the Dashboard
  "is_billing_reminders_enabled": false,    // Enable Billing Reminder
  "is_money_shown_on_evaluation": false,    // Show monetary Value on Evaluation
  "default_language_id": 2,       // Company Language: 1 for German, 2 for English, or Custom Language
  "is_absence_edit_enabled": true // Edit Absences allowed
  }
```

## Custom Field

A `custom field` describes an additional information about a `user`. They will be added to the `user’s` `employment` information. Only stakeholders can add `custom fields`.

- See <https://shyftplan.com/swagger#!/custom_fields/getApiV1CustomFields> for fetching all custom fields.
- See <https://shyftplan.com/swagger#!/custom_fields/postApiV1CustomFields> to create new custom field.
- See <https://shyftplan.com/swagger#!/custom_fields/deleteApiV1CustomFieldsId> for deleting an existing custom field.
- See <https://shyftplan.com/swagger#!/custom_fields/getApiV1CustomFieldsId> for fetching an existing custom field.
- See <https://shyftplan.com/swagger#!/custom_fields/putApiV1CustomFieldsId> for editing an existing custom field.

Example `Custom` `Field` json:

```js
{
  "id": 123,                      // Id of Custom Field
  "name": „Drivers License“,      // Name of Custom Field
  "company_id": 1337,             // Id of Company		
  "custom_field_type": "String",  // Type is alway a string of characters
  "sort": 1,                      // Sort Position of Costume Field in User Modal
  "created_at": "",
  "updated_at": ""                // Timestamps
}
```


## Employment

Every `user` in a `company` has an `employment`. It contains the name of the employee and whether he is employee or stakeholder in this company.
Optionally work hours, pay information and contact information can be provided.

- See <https://shyftplan.com/swagger#!/employments/getApiV1Employments> for fetching employments.
- See <https://shyftplan.com/swagger#!/users/postApiV1Users> to create an employment.
- See <https://shyftplan.com/swagger#!/employments/deleteApiV1EmploymentsId> to (soft) delete an employment.
- See <https://beta.shyftplan.com/swagger#!/employments/getApiV1EmploymentsId> for fetching an existing employment.
- See <https://beta.shyftplan.com/swagger#!/employments/getApiV1EmploymentsIdLiveInfo> for fetching live info on an employment.
- See <https://beta.shyftplan.com/swagger#!/employments/postApiV1EmploymentsIdRestoreEmployment> to restore a soft deleted employment.
- See <https://beta.shyftplan.com/swagger#!/employments/postApiV1EmploymentsIdDestroyEmployment> to (permanently) delete an employment.

The `employment` contains all information about the way the employee's
`paygrade types` are defined on a `company` level, within the `position` or employee information.
Either way they are being documented within the `employment` information.

- See <https://beta.shyftplan.com/swagger#!/employments/getApiV1EmploymentsIdPaygrades> for fetching an employment’s paygrades.
- See <https://beta.shyftplan.com/swagger#!/employments/getApiV1EmploymentsIdPaygradesPaygradeId> for fetching an existing paygrade.
- See <https://beta.shyftplan.com/swagger#!/employments/postApiV1EmploymentsIdPaygrades> to create paygrade for an employment.
- See <https://beta.shyftplan.com/swagger#!/employments/deleteApiV1EmploymentsIdPaygradesPaygradeId> to delete an existing paygrade of an employment.
- See <https://beta.shyftplan.com/swagger#!/employments/putApiV1EmploymentsIdPaygradesPaygradeId> to change an existing paygrade of an employment.


Example `Employment` json:

```js
{
  "id": 1337,             // Id of Employment
  "user_id": 4711,        // Id of User
  "company_id": 54,       // Id of Company
  "is_employee": true,    // Is the employment an employee?
  "is_stakeholder": true, // Is the employment an stakeholder?
  "created_at": ...,
  "updated_at": ...,      // Timestamps
  "deleted_at": ...,
  "first_name": "Foo",    // First Name
  "last_name": "Bar",     // Last Name
  "picture_data": {
    ...
  }
},
```

## Employment Survey

Every `employment` has an `employment` `survey`. It contains all specified employee information. To enable an employment survey the employment’s billing type `(payroll type)` needs to be set to billing. After the `payroll type` is set the employee is able to fill out the staff questionnaire which will be added to the `employment survey`.ddd

- See <https://shyftplan.com/swagger#!/employments_surveys/getApiV1EmploymentsSurveysId> for fetching employment survey.
- See <https://shyftplan.com/swagger#!/employments_surveys/postApiV1EmploymentsSurveysId> to create a employment survey.


Example `Employment Survey` json:

```js
{
  "id": 1337,             // Id of Employment Survey
  "first_name": "Foo",    // First Name of Employee
  "middle_name": null,    // Middle Name of Employee
  "last_name": "Bar",     // Last Name of Employee
  "date_of_entry": ...,
  "birth_date": ...,      // Timestamps
  "nationality": German,  // Nationality of the Employee
  "gender": Female,       // Gender
  "has_children": true,   // Has the Employee Children?
  "children_count": 1,    // How many Children does the Employee have?
  "exit_date": null,      // Exit Date if Employee has a Fixed-Term Contract
  "staff_number": "",     // Note that Staff Number is not equal to Punch Time Token, but set manually by stakeholder
  "phone_number": "",     // Phone Number of Employee
	...,                  // etc.
}
```

## Position

Every `company` can have multiple `positions`. `Positions` can be associated with several `locations` of a `company`.

- See <https://shyftplan.com/swagger#!/positions/getApiV1Positions> for fetching positions.
- See <https://shyftplan.com/swagger#!/positions/postApiV1Positions> to create a position.
- See <https://shyftplan.com/swagger#!/positions/deleteApiV1PositionsId> to (soft) delete an position.

Example `Position` json:

```js
{
  "id": 1337,             // Id of Position
  "company_id": 54,       // Id of Company
  "name": "Foo",          // Position Name
  "description": "",      // Description
  "color": "#aaeee1",     // Color shown in Web
  "created_at": ...,
  "updated_at": ...,      // Timestamps
  "deleted_at": ...,
  "sort": 13,             // Order Number
  "text_color": "#000",   // Foreground color (black or white)
  "note": ""              // Position Note is displayed on Shift
}
```

## Location

Every `company` can have multiple `locations`. `Locations` are meant to represent the actual sites of a company.

- See <https://shyftplan.com/swagger#!/locations/getApiV1Locations> for fetching locations.
- See <https://shyftplan.com/swagger#!/locations/postApiV1Locations> to create a location.
- See <https://shyftplan.com/swagger#!/locations/deleteApiV1LocationsId> to (soft) delete an location.

Example `Location` json:

```js
{
  "id": 1337,             // Id of Location
  "company_id": 54,       // Id of Company
  "name": "Foo",          // Location Name
  "created_at": ...,
  "updated_at": ...,      // Timestamps
  "deleted_at": ...,
  "sort": 2               // Order Number
}
```

## Locations Position

`LocationsPosition` is the join table of `locations` and `positions`. It references to a certain `position` existing in a certain `location.`

- See <https://shyftplan.com/swagger#!/locations_positions/getApiV1LocationsPositions> for fetching locations positions.
- See <https://shyftplan.com/swagger#!/locations_positions/postApiV1LocationsPositions> to create a locations position.
- See <https://shyftplan.com/swagger#!/locations_positions/deleteApiV1LocationsPositionsId> to (permanently) delete an locations position.

Example `LocationsPosition` json:

```js
{
  "id": 1337,             // Id of LocationsPosition
  "location_id": 4711,    // Id of Location
  "position_id": 815,     // Id of Position
  "created_at": ...,
  "updated_at": ...,      // Timestamps
  "deleted_at": ...,
  "sort": 3               // Used to sort them by increasing number
}
```

## Employments Position

`EmploymentsPosition` is the join table of `employments` and `LocationsPosition`.
This defines on which `positions` at what `locations` an employee is able to work.

- See <https://shyftplan.com/swagger#!/employments_positions/getApiV1EmploymentsPositions> for fetching employments positions.
- See <https://shyftplan.com/swagger#!/employments_positions/postApiV1EmploymentsPositions> to create a employments position.
- See <https://shyftplan.com/swagger#!/employments_positions/deleteApiV1EmploymentsPositionsId> to (permanently) delete an employments position.

Example `EmploymentsPosition` json:

```js
{
  "id": 1337,             // Id of EmploymentsPosition
  "employment_id": 4711,  // Id of Employment
  "locations_position_id": 815, // Id of LocationsPosition
  "created_at": ...,
  "updated_at": ...       // Timestamps
}
```

## Shiftplan

A `shiftplan` contains a collection of `shifts` within a defined period of time and is linked to an `location`.

- See <https://shyftplan.com/swagger#!/shiftplans/getApiV1Shiftplans> for fetching shiftplans.
- See <https://shyftplan.com/swagger#!/shiftplans/postApiV1Shiftplans> to create a shiftplan.
- See <https://shyftplan.com/swagger#!/shiftplans/deleteApiV1ShiftplansId> to (permanently) delete an shiftplan.

Example `Shiftplan` json:

```js
{
  "id": 1337,             // Id of Shiftplan
  "location_id": 58,      // Id of Location
  "starts_at": "yyyy-mm-dd", // Shiftplan goes from this date
  "ends_at": "yyyy-mm-dd",   // to this date (both included)
  "state": "published",   // State of shiftplan (published/unpublished)
  "name": "Foo",          // Shiftplan name
  "created_at": ...,
  "updated_at": ...,      // Timestamps
}
```

## Shift

A `shift` defines a working slot for a defined amount of `employments` at a `position` on a `shiftplan`.
Please note: The assigned `position` is set by the `LocationsPosition` where the `location` needs to match the very same the `shiftplan` is linked to.

- See <https://shyftplan.com/swagger#!/shifts/getApiV1Shifts> for fetching an existing shift.
- See <https://shyftplan.com/swagger#!/shifts/getApiV1ShiftsId> to create a shift.
- See <https://shyftplan.com/swagger#!/shifts/postApiV1Shifts> to create a shift.
- See <https://shyftplan.com/swagger#!/shifts/deleteApiV1ShiftsId> to (permanently) delete a shift.
- See <https://shyftplan.com/swagger#!/shifts/putApiV1ShiftsId> to update an existing shift.
- See <https://shyftplan.com/swagger#!/shifts/getApiV1ShiftsIdEvaluations> for fetching the evaluation of a shift.

Example `Shift` json:

```js
{
  "id": 1337,             // Id of Shift
  "shiftplan_id": 4711,   // Id of Shiftplan
  "locations_position_id": 815, //Id of LocationsPosition
  "starts_at": "...",     // Shift goes from this Time …
  "ends_at": "...",       // … to this Time (ISO 8601 format)
  "workers": 1,           // Maximum Amount of Employees assigned to this Shift
  "created_at": ...,
  "updated_at": ...,      
  "deleted_at": ...,      // Timestamps
  "break_time": 0,        // Duration of Breaks (in minutes)
  "can_evaluate": true,   // Can Shift be evaluated by the Employee?
  "note": null,           // Shift Note
  "untimed": false,       // If Shift Time should be accounted or not ???@Tamino
  "manager_note": null    // Manager Note (can just be seen by other Stakeholders)
}
```

## Staff Shift

A `staff shift` defines the assignment of an `employment` to a `shift`.

- See <https://shyftplan.com/swagger#!/staff_shifts/getApiV1StaffShifts> for fetching staff shifts.
- See <https://shyftplan.com/swagger#!/staff_shifts/postApiV1StaffShifts> to create a staff shift.
- See <https://shyftplan.com/swagger#!/staff_shifts/deleteApiV1StaffShiftsId> to (permanently) delete an staff shift.
- See <https://shyftplan.com/swagger#!/staff_shifts/getApiV1StaffShiftsId> for fetching an existing staff shift.

Example `Staff Shift` json:

```js
{
  "id": 1337,             // Id of StaffShift
  "shift_id": 4711,       // Id of Shift
  "employment_id": 815,   // Id of Employment
  "state": "no_evaluation", // Current Evaluation state
                            // Possible values:
                            // - no_evaluation
                            // - done_evaluation
                            // - needs_evaluation
                            // - punchtimed
                            // - no_show
  "created_at": ...,
  "updated_at": ...,      
  "deleted_at": ...,      // Timestamps
  "total_minutes": 241,   // Duration of the working time (without break)
  "total_payment": 50.21  // Sum of all Payments
}
```

## Request

A `request` defines an employee's wish to make a change to a `shift`. If an employee is not currently assigned to a `shift` he can request an application. If an employee is currently assigned to a `shift` he can request a change.

- See <https://shyftplan.com/swagger#!/requests/getApiV1Requests> for fetching requests.
- See <https://shyftplan.com/swagger#!/requests/getApiV1RequestsId> for fetching an existing request.
- See <https://shyftplan.com/swagger#!/requests/deleteApiV1RequestsId> to (permanently) delete a request.
- See <https://shyftplan.com/swagger#!/requests/postApiV1RequestsApplyShift> for applying to a shift.
- See <https://shyftplan.com/swagger#!/requests/postApiV1RequestsConfirmApplyShift> to confirm a shift application.
- See <https://shyftplan.com/swagger#!/requests/postApiV1RequestsChangeShift> to create a change request.


Example `Request` json:

```js
{
  "id": 1337,             // Id of Request
  "shift_id": 4711,       // Id of Shift
  "employment_id": 815,   // Id of Employment
  "created_at": ...,
  "updated_at": ...,      // Timestamps
  "type": "StaffRequest"  // Type of Request (StaffRequest/ChangeRequest)
}
```

## Evaluation

When a `shift` is performed by an employee an `evaluation` is automatically provided. This can be manually adjusted by stakeholders or a proposal of changes can be made by the employee (if enabled).
Please note: payments can be included for the `evaluation` response. This decreases the response time and requires owner rights for the API user.

- See <https://shyftplan.com/swagger#!/evaluations/getApiV1Evaluations> for fetching evaluations.
- See <https://shyftplan.com/swagger#!/evaluations/getApiV1EvaluationsStaffShiftId> for fetching an evaluation for a staff shift.
- See <https://shyftplan.com/swagger#!/evaluations/postApiV1EvaluationsStaffShiftId> to set an evaluation for a staff shift.
- See <https://shyftplan.com/swagger#!/evaluations/postApiV1EvaluationsStaffShiftIdDidNotShow> to set a staff shift to employee did not show.
- See <https://shyftplan.com/swagger#!/evaluations/postApiV1EvaluationsStaffShiftIdDidShow> to set a staff shift to employee did show.
- See <https://shyftplan.com/swagger#!/evaluations/getApiV1EvaluationsStaffShiftIdPayments> for fetching payments for an evaluation.
- See <https://shyftplan.com/swagger#!/evaluations/postApiV1EvaluationsStaffShiftIdPayments> to create payments for an evaluation.
- See <https://shyftplan.com/swagger#!/evaluations/deleteApiV1EvaluationsStaffShiftIdPaymentsPaymentId> to delete payment of an evaluation.
- See <https://shyftplan.com/swagger#!/evaluations/getApiV1EvaluationsStaffShiftIdPaymentsPaymentId> for fetching an existing payment of an evaluation.
- See <https://shyftplan.com/swagger#!/evaluations/putApiV1EvaluationsStaffShiftIdPaymentsPaymentId> to update an existing payment of an evaluation.

Example `Evaluation` json:

```js
{
  "id": 1337,             // Id of StaffShift
  "shift_id": 4711,       // Id of Shift
  "employment_id": 815,   // Id of Employment
  "locations_position_id": 54, // Id of LocationsPosition
  "shiftplan_id": 136,    // Id of Shiftplan
  "location_id": 58,      // Id of Location
  "position_id": 96,      // Id of Position
  "user_id": 405,         // Id of User
  "first_name": "Foo",    // First Name of Employment
  "last_name": "Bar",     // Last Name of Employment
  "evaluation_starts_at": ..., // Evaluated Start of Shift (ISO 8601)
  "evaluation_ends_at": ...,   // Evaluated End of Shift (ISO 8601)
  "evaluation_break": 0,       // Break Time in Minutes
  "evaluation_duration": 330,  // Working Time in Minutes
  "state": "no_evaluation",    // State of Evaluation (see StaffShift)
  "shift_note": null,    	 // Shift note
  "admin_evaluation_note": null,    // Evaluation Note by Stakeholder
  "employee_evaluation_note": null, // Evaluation Note by Employee
  "created_at": ...,
  "updated_at": ...,     // Timestamps
  "position": { ...
    "name": "Foo"        // Position Name
  },
  "location": {
    "name": "Bar"        // Location Name
  },
  "shift": {
    "starts_at": ...,    // Planned Shift Start
    "ends_at": ...,      // Planned Shift End
    "break_time": 0      // Planned Shift Break Time
  }
}
```

## Language

Every `company` can set their `default language`, as well as `users` individually. Optionally it is possible to add custom `languages` and create `translations`.

- See <https://shyftplan.com/swagger#!/languages/getApiV1Languages> for fetching all languages.


Example `Language` json:

```js
{
"items": [
   {
      "id": 1,              // Id of Language
      "company_id": null,   // Default Languages German and English are not bound to one single Company
      "name": "Deutsch",    // Name of Language in its Language
      "locale": "de",       // Acronym
      "fallback": "de"      // Fallback Language when translation for chosen Language is not set
    },
    {
      "id": 2,
      "company_id": null,
      "name": "English",
      "locale": "en",
      "fallback": "en"
    }
  ],
  "total": 2,		// Add a Custom Language for more Options via Translations
}
```

## Live Staff Shift

??? @tamino : is my definition correct?

A `live staff shift` contains all information about a performed `shift`. Note that only `company` owners or stakeholders with `view rights` can access `live staff shift` information.

- See <https://shyftplan.com/swagger#!/live_staff_shifts/getApiV1LiveStaffShifts> for fetching all live staff shifts.


Example `Live Staff Shift` json:

```js
{
  "id": 1337,                   // Id of Live Staff Shift
  "staff_shift_id": 4771,       // Id of Staff Shift
  "state": "needs_evaluation",  // State of Evaluation
                                  // Possible values:
                                  // - no_evaluation
                                  // - done_evaluation
                                  // - needs_evaluation
                                  // - punchtimed
                                  // - no_show
  "employment_id": 3317,        // Id of Employment
  "shift_id": 1447,             // Id of Shift		
  "locations_position_id": 4471,  // Id of LocationsPosistion
  "shiftplan_id": 4147,         // Id of Shiftplan
  "position_id": 7741,          // Id of Position
  "location_id": 3731,          // Id of Location
  "company_id": 1137,           // Id of Companya
  "location_name": "",          // Name of Location
  "position_name": "",          // Name of Position
  "position_color": "",         // Position Color
  "position_text_color": "#000",  // Color of Position is either Black or White
  "user_id": 4417,              // Id of User
  "punch_timing_id": null,      // Id of Punch Timing
  "punch_timing_end_time": "",		
  "punch_timing_overtime_start": "",	  
  "punch_break_id": null,       // Only if Shift has been punched via Punch Time
  "shift_ends_at": "",
  "shift_starts_at": "",        // Timestamps
  "first_name": "Foo",          // First Name of Employee		
  "last_name": "Bar",           // Last Name of Employee
  "live_status": "idle"         // ??? @tamino
}
```

## Location

Every `company` and `position` has at least one `location`. The join table `LocationsPosition` shows which `position` is associated with which `location`.

- See <https://shyftplan.com/swagger#!/locations/getApiV1Locations> for fetching all locations.
- See <https://shyftplan.com/swagger#!/locations/postApiV1Locations> to create a new location.
- See <https://shyftplan.com/swagger#!/locations/deleteApiV1LocationsId> to (soft) delete an existing location.
- See <https://shyftplan.com/swagger#!/locations/getApiV1LocationsId> for fetching an existing location.
- See <https://shyftplan.com/swagger#!/locations/putApiV1LocationsId> to edit an existing location.
- See <https://shyftplan.com/swagger#!/locations/postApiV1LocationsIdDestroyLocation> to (permanently) delete an existing location.


Example `Location` json:

```js
{
  "items": [
    {
      "id": 1337,           // Id of Location
      "name": "Zentrale",   // Name of Location
      "company_id": 4471,   // Id of Company
      "created_at": "",
      "updated_at": "",		
      "deleted_at": "",     // Timestamps
      "sort": 1,            // Locations are sorted by increasing Numbers
      "action_type": ""     // ??? @tamino
    }
  ],
  "total": 1
}
```

## Login

`Email` and `password` are the credentials for the `login`. To get an `authentication token` for using the API a successful `login` is required.

- See <https://shyftplan.com/swagger#!/login/postApiV1Login> to create a login.


Example `Login` json:

```js
{
  "success": true,              // Was Login Action successful?
  "info": "Logged in",          // Login State
  "authentication_token": "",   // Automatically generated by Login
  "User": {                     // User Information (See User for more)
      ...,
  },
  "Company": {					// Company Information (See Company for more)
      ...,
  },
```

## Newsfeed

The `newsfeed` contains notifications about created `absences`, `evaluations`, `punch times`, `requests` and `shiftplans`. It is also possible to send messages via `newsfeed`.

- See <https://shyftplan.com/swagger#!/newsfeeds/getApiV1Newsfeeds> for fetching all newsfeeds.
- See <https://shyftplan.com/swagger#!/newsfeeds/postApiV1Newsfeeds> to create a newsfeed.



Example `Newsfeed` json:

```js
{
  "items": [
    {
      "id": 1337,                   // Id of Newsfeed
      "company_id": 54,             // Id of Company
      "user_id": 4471,              // Id of User
      "key": "shiftplan.published", // Key to Newsfeed Object
      "objekt_id": 13377,           // Id of Newsfeed Object
      "objekt_type": "Shiftplan",   // Type of Newsfeed Object
      "subjekt_id": null,           // Id of Subject
      "subjekt_type": null,         // Type of Subject
      "context_id": 58,             // Id of Context
      "context_type": "Location",   // Type of Context
      "message": null,              // If applicable this contains Message send via Newsfeed
      "created_at": "",
      "updated_at": "",             // Timestamps
      "company": {
        "name": "shyftplan",        // Name of Company
        "picture_data": {           // Avatar of Company
            ...,
        }
	 }
}
```

## Notification Configuration

`Notification configurations` define whether a `user` gets a notification via mobile or as an email automatically sent when a notification appears on the `newsfeed`.

- See <https://shyftplan.com/swagger#!/notification_configurations/getApiV1NotificationConfigurations> for fetching all notification configurations.
- See <https://shyftplan.com/swagger#!/notification_configurations/putApiV1NotificationConfigurationsId> to update a notification configuration.
- See <https://shyftplan.com/swagger#!/notification_configurations/getApiV1NotificationConfigurationsId> for fetching an existing notification configuration.


Example `Notification Configuration` json:

```js
{
  "items": [
    {
      "id": 1337,                   // Id of Noticifation Configuration
      "absences_mail": true,        // Should Absence Notifications be sent as an E-Mail?
      "absences_mobile": true,      // Should Absence Notifications be sent to the Users Mobile Device?
      "evaluations_mail": true,     // Should Evaluation Notifications be sent as an E-Mail?
      "evaluations_mobile": true,   // Should Evaluation Notifications be sent to the User’s Mobile Device?
      "messages_mobile": true,      // Should Messages be sent to the User’s Mobile Device?
      "stakeholder_requests_mail": true,    // Should Stakeholders receive Shift Requests as an E-Mail?
      "requests_mobile": true,              // Should Users receive accepted/declined Shift Request Notifications on their Mobile Device?
      "shiftplans_mobile": true,            // Should Shiftplan Notifications be sent to the User’s Mobile Device?
      "staff_messages_mobile": true,        // Should Staff Messages be sent to the User’s Mobile Device?
      "employment_id": 4471,             // Id of Employment
      "created_at": "",						
      "updated_at": "",                  // Timestamps
      "stakeholder_absences_mobile": true,      // Should Stakeholders receive Absence Requests on their Mobile Device?
      "stakeholder_evaluations_mobile": true,   // Should Stakeholders receive Evaluation Suggestions on their Mobile Device?
      "stakeholder_requests_mobile": true,      // Should Stakeholders receive Evaluation Suggestions on their Mobile Device?
      "stakeholder_auto_punchout_mobile": true, // Should Stakeholders receive Auto Punch Out Notifications on their Mobile Device?
      "auto_punchout_mobile": true,             // Should Users receive Auto Punch Out Notifications on their Mobile Device?
      "stakeholder_absences_mail": true,        // Should Stakeholders receive Absence Notifications as an E-Mail?
      "stakeholder_auto_request_change_accept_mobile": true,  // Should Stakeholders receive Auto Shift Change Requests on their Mobile Device?
      "stakeholder_auto_request_change_accept_mail": true,    // Should Stakeholders receive Auto Shift Change Requests as an E-Mail?
      "stakeholder_shift_application_mobile": true,           // Should Stakeholders receive Shift Applications on their Mobile Device?
      "stakeholder_shift_application_mail": true,      // Should Stakeholders receive Shift Applications as an E-Mail?
      "application_request_refused_mail": true,       // Should Users receive Application Request Refusals as an E-Mail?
      "application_request_refused_mobile": true      // Should Users receive Application Request Refusals on their Mobile Device?
    }
  ],
  "total": 7,
}
```

## Paygrade Type

`Paygrade types` represent different salary and compensation models a company can have designated to use for an employees `payment`.

They are specified on `company` level, `position` level or employee level. `Paygrades` set on `position` level will override `paygrades` on `company` level, `paygrades` set on employee level will override `paygrades` on `company` and `position` level.
Please keep in mind that it is not possible to set more than one `paygrade` for an employee with same `paygrade type` and same `position`.

- See <https://shyftplan.com/swagger#!/paygrade_types/getApiV1PaygradeTypes> for fetching all paygrade types.
- See <https://shyftplan.com/swagger#!/paygrade_types/getApiV1PaygradeTypesId> for fetching an existing paygrade type.


Example `Paygrade Type` json:

```js
{
 "items":[
  {
   "id": 1337,          // Id of Paygrade Type
   "name": "Regular",   // Type of Pay (Fixed/Bonus Fixed/Hourly/Bonus Percentage)
   "pay_type": "fixed", // Type of Pay (Fixed/Bonus Fixed/Hourly/Bonus Percentage)
   "company_id": ,      // Id of Company
   "deleted_at": "",
   "created_at": "",
   "updated_at": "",		
   "start_time": null,
   "end_time": null,    // Timestamps
   "special_day": false,  // Does this Paygrade Type apply for a Special Day e.g. a Holiday?
   "monday": true,		
   "tuesday": true,
   "wednesday": true,
   "thursday": true,
   "friday": true,
   "saturday": false,
   "sunday": false,   // Days of Validation
   "min_minutes": 0,  // Information about Paygrade Validation in Minutes
   "selection": null,   // ??? @tamino How many times is this Paygrade Type applied?
   "paychex_la": null,  // ??? @tamino How many Paychex have been calculated with this Paygrade Type?
  }
```


## Payment

`Payments` define the total amount of money an `employee` earns from a `shift.`
Note that to be able to receive `payment` information about an employee's performed `shifts the `staff shift id` is required.

- See <https://shyftplan.com/swagger#!/staff_shifts/getApiV1StaffShifts> to get the staff shift id required for payment requests.
- See <https://shyftplan.com/swagger#!/payments/getApiV1PaymentsStaffShiftId> for fetching an existing payment for a staff shift.


Example `Payment` json:

```js
{
  "id": 1337,                 // Id of Payment
  "type": "AdminPayment",     // Type of Payment ??? @tamino
  "value": 10,                // Value of Paygrade Type
  "paygrade_type_id": 7331,   // Id of Paygrade Type
  "is_edited": false,         // Is the Payment editable?
  "inherited_from": „company“,      // Where has the Paygrade been created? (Company/Position/Employee)
  "display_name": „regular, fixed“, // Display name is put together from Name of Paygrade and Type of Paygrade
  "sum_with_shift": 480,      // Length of Shift in Minutes
  "paygrade_type": {		
    "id": 6844,               // Id of Paygrade Type
    "name": „regular“,        // Name of Paygrade Type
    "pay_type": „fixed“       // Type of Paygrade
    }
  },
```


## Punch Timing

`Punch Timings` define punched start and end time of a shift, as well as punched breaks. `Punch Timing` a can be activated on any mobile device with the shyftplan app and on <https://shyftplan.com/punch_timings>.

- See <https://shyftplan.com/swagger#!/punch_timings/postApiV1PunchTimingsStart> for starting punch time session.
- See <https://shyftplan.com/swagger#!/punch_timings/postApiV1PunchTimingsShiftCreate> for creating a shift via punch time.
- See <https://shyftplan.com/swagger#!/punch_timings/putApiV1PunchTimingsId> for updating punch time.
- See <https://shyftplan.com/swagger#!/punch_timings/postApiV1PunchTimingsPunchTimingIdPunchBreaks> to start break.
- See <https://shyftplan.com/swagger#!/punch_timings/putApiV1PunchTimingsPunchTimingIdPunchBreaksId> to end break.
- See <https://shyftplan.com/swagger#!/punch_timings/getApiV1PunchTimings> for fetching current punch time company.
- See <https://shyftplan.com/swagger#!/punch_timings/getApiV1PunchTimingsFetchLocationsPositions> for fetching location positions.
- See <https://shyftplan.com/swagger#!/punch_timings/getApiV1PunchTimingsPunchTime> for fetching all punch ins. // is that correct? ??? @tamino
- See <https://shyftplan.com/swagger#!/punch_timings/getApiV1PunchTimingsRecord> for fetching status.

Example `Punch Timing` json:

```js
{
  "success": true,		// Punch Timings Status
  "company_name": ""	// Name of Company
}
```

## Right

`Rights` define the way a `user` is able to use shyftplan. `Rights` can be individually assigned to a `user`. Only owners in any case and stakeholders with the same `rights` they want to assign to a `user` can do so. Choosing the accurate `rights` for a stakeholder is depending on `company` internal agreements about their operational area.

Rights associated to `shifts` can be listed as `view rights` and `manage rights`. These `rights` are conjoint like this:

 Assigning `view rights` doesn’t affect `manage rights`.
 Deleting `view rights` affect `manage rights`.
 Assigning `manage rights` affect `view rights`.
 Deleting `manage rights` doesn’t affect `view rights`.

- See <https://shyftplan.com/swagger#!/rights/getApiV1Rights> for fetching all rights.
- See <https://shyftplan.com/swagger#!/rights/getApiV1RightsMy> for fetching all rights applying to user.
- See <https://shyftplan.com/swagger#!/rights/deleteApiV1RightsId> to take away a right from an employment.
- See <https://shyftplan.com/swagger#!/rights/putApiV1RightsId> to assign a right to an employment.

For each `location` shifts can either be viewable or hidden. All `positions` within said `location` are being considered, also when deleting this right.

- See <https://shyftplan.com/swagger#!/rights/deleteApiV1RightsLocationShiftShowRight> to delete a user’s right to view all shifts of a location.
- See <https://shyftplan.com/swagger#!/rights/putApiV1RightsLocationShiftShowRight> to assign the right to view all shifts of a location to a user.

While regarding each `position` individually within a `location` shifts can either be viewable or hidden.

- See <https://shyftplan.com/swagger#!/rights/deleteApiV1RightsLocationsPositionShiftShowRight> to delete a user’s right to view a certain position of a location.
- See <https://shyftplan.com/swagger#!/rights/putApiV1RightsLocationsPositionShiftShowRight> to assign the right to view a certain position of a location to a user.

For each `location` `shifts` can be managed. This means a `shift` is not only visible to the stakeholder, but can be edited, removed and created for all `employees` working at the applied `location`.
All `position` within said `location` are being considered. All `manage shift rights` translate to matching `view shift right` when they are being assigned. Removing a `location` from manage right this `location` is still viewable.

- See <https://shyftplan.com/swagger#!/rights/deleteApiV1RightsLocationShiftManageRight> to delete a user’s right to manage all shifts of a location.
- See <https://shyftplan.com/swagger#!/rights/putApiV1RightsLocationShiftManageRight> to assign the right to manage all shifts of a location to a user.

While regarding each `position` individually within a `location` shifts can also be managed.

- See <https://shyftplan.com/swagger#!/rights/deleteApiV1RightsLocationsPositionShiftManageRight> to delete a user’s right to manage a certain position of a location.
- See <https://https://shyftplan.com/swagger#!/rights/putApiV1RightsLocationsPositionShiftManageRight> to assign the right to manage a certain position of a location to a user.

Every shift has a `payment` that can be viewed if necessary.

- See <https://shyftplan.com/swagger#!/rights/deleteApiV1RightsLocationPaymentShowRight> to delete a user’s right to view payments for all shifts of a location.
- See <https://shyftplan.com/swagger#!/rights/putApiV1RightsLocationPaymentShowRight> to assign the right to view payments for all shifts of a location to a user.

While regarding each `position` individually within a `location`, `payments` for shifts of said `position` can be viewed.

- See <https://shyftplan.com/swagger#!/rights/deleteApiV1RightsLocationsPositionPaymentShowRight> to delete a user’s right to view payments for all shifts in a certain position of a location.
- See <https://shyftplan.com/swagger#!/rights/putApiV1RightsLocationsPositionPaymentShowRight> to assign the right to view payments for all shifts in a certain position of a location to a user.

Every shift has a `payment` that can be managed by a stakeholder. While managing `payments` within a `location` the `view right` for this `location` are automatically assigned as well. While when deleting the `manage right`, the `view right` for `payments` still remains until this right is being deleted individually.  

- See <https://shyftplan.com/swagger#!/rights/deleteApiV1RightsLocationPaymentManageRight> to delete a user’s right to manage payments for all shifts in a certain position of a location
- See <https://shyftplan.com/swagger#!/rights/putApiV1RightsLocationPaymentManageRight> to assign the right to manage payments for all shifts in a certain position of a location to a user.

While regarding each `position` individually within a `location`, `payments` for shifts of said position can be managed.

- See <https://shyftplan.com/swagger#!/rights/deleteApiV1RightsLocationsPositionPaymentManageRight> to delete a user’s right to manage payments for a certain position of a location.
- See <https://shyftplan.com/swagger#!/rights/putApiV1RightsLocationsPositionPaymentManageRight> to assign the right to manage  payments for a certain position of a location to a user.


Example `Right` json:

```js
{
  "id": 4277,           // Id of Right
  "context_id": 1337,   // Id of Context
  "context_type": "",   // Type of Context (e.g. Location/Position/Punch Time etc.)
  "name": "",           // Name of Right (e.g. location_“locationId“_show_right)
  "company_id": 1147,   // Id of Company
  "created_at": "",
  "updated_at": "",     // Timestamps
  "is_possible_to_set": true   // Is this Right possible to set?
}
```

## Translation

`Translations` demonstrate key-value pairs that can be added to a `custom language`. In order to use `translations` it is mandatory to create a `custom language`.
While the key for the translation is a given value, the translation is arbitrary. This way it is possible to generate a new dictionary for the shyftplan software or customize terms and definitions according to the companies internal structure.   

- See <https://beta.shyftplan.com/swagger#!/translations/getApiV1Translations> for fetching all translations.
- See <https://beta.shyftplan.com/swagger#!/translations/postApiV1Translations> to create a translation.
- See <https://beta.shyftplan.com/swagger#!/translations/deleteApiV1TranslationsId> to delete a translation.
- See <https://beta.shyftplan.com/swagger#!/translations/getApiV1TranslationsId> for fetching an existing translation.
- See <https://beta.shyftplan.com/swagger#!/translations/putApiV1TranslationsId> to edit an existing translation.



Example `Translation` json:

```js
{
  "id": 1147,         // Id of Translation
  "language_id": 3,   // Id of Language
  "key": "",          // Keys are listed when fetching all Translations
  "value": ""         // new desired Translation
}
```


## User


A `user` can be an employee, a stakeholder and/or the company owner. As soon as someone is invited to shyftplan a new `user` has been created.
`Users` can be associated with several `companies`.
To delete a `user` it has to be done by deleting the `user’s` `employment` associated with the particular company the `user` is leaving.


- See <https://beta.shyftplan.com/swagger#!/users/getApiV1Users> for fetching all users.
- See <https://beta.shyftplan.com/swagger#!/users/postApiV1Users> to add a user employment.
- See <https://beta.shyftplan.com/swagger#!/users/postApiV1UsersChangeLocale> to change locale.
- See <https://beta.shyftplan.com/swagger#!/users/getApiV1UsersId> for fetching an existing user.
- See <https://beta.shyftplan.com/swagger#!/users/putApiV1UsersId> to edit an existing user employment.


Example `User` json:

```js
{
  "id": 3313,         // Id of User
  "email": "",        // Email-address of the User
  "created_at": ...,
  "updated_at": ...,  // Timestamps
  "current_company_id": 54,       // Id of current Company the User logged in to
  "current_location_id": 4711,    // Id of current Location the User has selected
  "invitation_created_at": ...,
  "invitation_sent_at": ...,
  "invitation_accepted_at": ...,  // Timestamps
  "invited_by_id": "",    // Id of User that sent the invite
  "invited_by_type": ,    // Type of User that sent the invite (stakeholder/owner)
  "phone_number": 03088789603,  // Phone Number of User
  "deleted_at": "",             // Timestamp
  "locale": "",                 // ??? @tamino
  "is_inactive": false,         // Is the User inactive?
}
```
