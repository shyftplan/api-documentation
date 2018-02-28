# shyftplan API documentation

All available REST API urls and parameters are listed and described at <https://shyftplan.com/swagger>.

Every API call needs authentication with the params `user_email` and `authentication_token`.
The email is the one used for signing in into shyftplan. It is recommended to create a separate Login for using the API.

The authentication token can be aquired by two ways:

- Create an Authentication Token for your current employment on your Profile
- [POST] <https://shyftplan.com/api/v1/login?user[email]=email&user[password]=password>

  where `email` and `passord` are the credentials to log into shyftplan
  The response contains the Authentication token
  ```js
  {
    "success": true,
    "info": "Logged in",
    "authentication_token": ":token:",
    ...
  }
  ```

## List Actions

Any read list action is paginated with the params `page` (page number)  and `per_page` (amount of records per page). The default page size is `1000` for most of the actions.

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
That parameters can't be passed by the swagger UI. If you want to use them you need to pass them like `?ids[]=1&ids[]=2&ids[]=3`

## Employment

Every `User` in a `Company` has an `Employment`. It contains the name on the Employee and whether he is employee or stakeholder in his company.

- See <https://shyftplan.com/swagger#!/employments/getApiV1Employments> for fetching employments.
- See <https://shyftplan.com/swagger#!/users/postApiV1Users> to create a employment.
- See <https://shyftplan.com/swagger#!/employments/deleteApiV1EmploymentsId> to (soft) delete an employment.

Example `Employment` json:

```js
{
  "id": 1337,             // Id of Position
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

## Position

Every `Company` can have multiple `Position`s.

- See <https://shyftplan.com/swagger#!/positions/getApiV1Positions> for fetching positions.
- See <https://shyftplan.com/swagger#!/positions/postApiV1Positions> to create a position.
- See <https://shyftplan.com/swagger#!/positions/deleteApiV1PositionsId> to (soft) delete an position.

Example `Position` json:

```js
{
  "id": 1337,             // Id of Position
  "company_id": 54,       // Id of Company
  "name": "Foo",          // Position Name
  "description": "",      // Descripton
  "color": "#aaeee1",     // Color shown in web
  "created_at": ...,
  "updated_at": ...,      // Timestamps
  "deleted_at": ...,
  "sort": 13,             // Used to order them
  "text_color": "#000",   // Foreground color (black or white)
  "note": ""              // Position Note
},
```

## Location

Every `Company` can have multiple `Location`s.

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
  "sort": 2               // Used to order them
},
```

## LocationsPositon

`LocationsPosition` is the join table of `Location`s and `Position`s.

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
  "sort": 3               // Used to order them
},
```

## EmploymentsPosition

`EmploymentsPosition` is the join table of `Employment`s and `LocationsPosition`s.
This defines on which positions and locations an employee is able to work.

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
},
```

## Shiftplan

A shiftplan contains a collections of `Shift`s assigned to it and is related to an `Location`.

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
},
```

## Shift

A `Shift` defines a working slot for an defined amount of `Employment`s at an `Position` on a `Shiftplan`.
Please note: The assigned position is set by the `LocationsPosition` where the `Location` needs to match the one from the `Shiftplan`.

- See <https://shyftplan.com/swagger#!/shifts/getApiV1Shifts> for fetching shifts.
- See <https://shyftplan.com/swagger#!/shifts/postApiV1Shifts> to create a shift.
- See <https://shyftplan.com/swagger#!/shifts/deleteApiV1ShiftsId> to (permanently) delete an shift.

Example `Shift` json:

```js
{
  "id": 1337,             // Id of Shift
  "shiftplan_id": 4711,   // Id of Shiftplan
  "locations_position_id": 815, // Id of LocationsPosition
  "starts_at": "...",     // Shift goes from this time
  "ends_at": "...",       // to this time (ISO 8601 format)
  "workers": 1,           // Maximum amount of workers on this shift
  "created_at": ...,
  "updated_at": ...,      // Timestamps
  "deleted_at": ...,
  "break_time": 0,        // Duration of breaks (in minutes)
  "can_evaluate": true,   // Can be evaluated by employees
  "note": null,           // Shift Note
  "untimed": false,       // If shift time should be accounte or not
  "manager_note": null    // Manager Note (can just be seen by other Stakeholders)
},
```

## StaffShift

A `StaffShift` defines the assignment of an `Employment` to a `Shift`.
Please note: The assigned position is set by the `LocationsPosition` where the `Location` needs to match the one from the `Shiftplan`.

- See <https://shyftplan.com/swagger#!/staff_shifts/getApiV1StaffShifts> for fetching staff shifts.
- See <https://shyftplan.com/swagger#!/staff_shifts/postApiV1StaffShifts> to create a staff shift.
- See <https://shyftplan.com/swagger#!/staff_shifts/deleteApiV1StaffShiftsId> to (permanently) delete an staff shift.

Example `StaffShift` json:

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
  "updated_at": ...,      // Timestamps
  "deleted_at": ...,
  "total_minutes": 241,   // Duration of the working time (without break)
  "total_payment": 50.21  // Sum of all payments
},
```

## Request

A `Request` defines a wish of an `Employment` to an shift.
If an employee is not currently assigned to a shift he can request an application.
If an employee is currently assigned to a shift he can request a change.

- See <https://shyftplan.com/swagger#!/requests/getApiV1Requests> for fetching requests.
- See <https://shyftplan.com/swagger#!/requests/deleteApiV1RequestsId> to (permanently) delete an request.

Example `Request` json:

```js
{
  "id": 1337,             // Id of Request
  "shift_id": 4711,       // Id of Shift
  "employment_id": 815,   // Id of Employment
  "created_at": ...,
  "updated_at": ...,      // Timestamps
  "type": "StaffRequest"  // Type of Request (StaffRequest/ChangeRequest)
},
```

## Evaluation

When a `Shift` is done by an `Employment` the shift has an `Evaluation` which can be adjusted by stakeholders or suggested by the employee (if enabled).
Please note: Payments can be included for the `Evaluation` response. This decreases the response time and requires Owner rights for the API user.

- See <https://shyftplan.com/swagger#!/evaluations/getApiV1Evaluations> for fetching evaluations.
- See <https://shyftplan.com/swagger#!/evaluations/postApiV1EvaluationsStaffShiftId> to update the evaluation.

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
  "evaluation_break": 0,       // Break time in minutes
  "evaluation_duration": 330,  // Working time in minutes
  "state": "no_evaluation",    // State of Evaluation (see StaffShift)
  "shift_note": null,    // Shift note
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
},
```
