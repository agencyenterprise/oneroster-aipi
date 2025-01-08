OneRoster Extension Specification Diff Document
This diff document highlights the additional capabilities and metadata-driven enhancements we are layering onto OneRoster v1.2 Services—introducing new functionality, routes, and data handling approaches—while preserving the integrity and interoperability of the original specification.

Service-Specific Endpoint Extensions
Rostering Service
The current OneRoster v1.2 spec for the Rostering Service provides the GET method for its defined endpoints. We will layer on top POST, PUT, and DELETE methods.

Base endpoint: /ims/oneroster/rostering/v1p2
OneRoster v1.2 Rostering Service Endpoints Extended

POST for /academicSessions

```typescript
{
  "sourcedId": "2024-fall-semester",      // OPTIONAL: auto-generated if not provided
  "status": "active",                     // OPTIONAL: defaults to "active"
  "title": "Fall Semester 2024",          // REQUIRED
  "type": "semester",                     // REQUIRED: must be term/semester/schoolYear/gradingPeriod
  "startDate": "2024-08-15",              // REQUIRED: ISO date string
  "endDate": "2024-12-20",                // REQUIRED: ISO date string
  "parent": {                             // OPTIONAL
    "sourcedId": "2024-school-year"       // REQUIRED if parent object is included
  },
  "schoolYear": "2024",                   // REQUIRED
  "org": {                                // REQUIRED
    "sourcedId": "school-123"             // REQUIRED
  }
}
```

PUT for /academicSessions/{sourcedId}

```typescript
{
  "title": "Fall Semester 2024",          // REQUIRED
  "type": "semester",                     // REQUIRED: must be term/semester/schoolYear/gradingPeriod
  "startDate": "2024-08-15",              // REQUIRED: ISO date string
  "endDate": "2024-12-20",                // REQUIRED: ISO date string
  "parent": {                             // OPTIONAL
    "sourcedId": "2024-school-year"       // REQUIRED if parent object is included
  },
  "schoolYear": "2024",                   // REQUIRED
  "org": {                                // REQUIRED
    "sourcedId": "school-123"             // REQUIRED
  }
}
```

DELETE for /academicSessions/{sourcedId}
The only requirement is the sourcedId in the URL path parameter.

POST for /terms

```typescript
{
  "sourcedId": "2024-spring-term",        // OPTIONAL: auto-generated if not provided
  "status": "active",                     // OPTIONAL: defaults to "active"
  "title": "Spring Term 2024",            // REQUIRED
  "type": "term",                         // REQUIRED: must be "term"
  "startDate": "2024-01-15",              // REQUIRED: ISO date string
  "endDate": "2024-05-30",                // REQUIRED: ISO date string
  "parent": {                             // OPTIONAL
    "sourcedId": "2024-school-year"       // REQUIRED if parent object is included
  },
  "schoolYear": "2024",                   // REQUIRED
  "org": {                                // REQUIRED
    "sourcedId": "school-123"             // REQUIRED
  }
}
```

PUT for /terms/{sourcedId}

```typescript
{
  "title": "Spring Term 2024",            // REQUIRED
  "type": "term",                         // REQUIRED: must be "term"
  "startDate": "2024-01-15",              // REQUIRED: ISO date string
  "endDate": "2024-05-30",                // REQUIRED: ISO date string
  "parent": {                             // OPTIONAL
    "sourcedId": "2024-school-year"       // REQUIRED if parent object is included
  },
  "schoolYear": "2024",                   // REQUIRED
  "org": {                                // REQUIRED
    "sourcedId": "school-123"             // REQUIRED
  }
}
```

DELETE for /terms/{sourcedId}
The only requirement is the sourcedId in the URL path parameter.

POST for /terms/{termSourcedId}/gradingPeriods

```typescript
{
  "sourcedId": "2024-q1",                 // OPTIONAL: auto-generated if not provided
  "status": "active",                     // OPTIONAL: defaults to "active"
  "title": "Q1 2024",                     // REQUIRED
  "type": "gradingPeriod",                // REQUIRED: must be "gradingPeriod"
  "startDate": "2024-01-15",              // REQUIRED: ISO date string
  "endDate": "2024-03-15",                // REQUIRED: ISO date string
  "parent": {                             // REQUIRED for grading periods
    "sourcedId": "{termSourcedId}"        // REQUIRED: must match URL parameter
  },
  "schoolYear": "2024",                   // REQUIRED
  "org": {                                // REQUIRED
    "sourcedId": "school-123"             // REQUIRED
  }
}
```

POST for /classes

```typescript
{
  "sourcedId": "class-123",              // OPTIONAL: auto-generated if not provided
  "status": "active",                    // OPTIONAL: defaults to "active"
  "title": "Algebra 101",                // REQUIRED
  "classCode": "ALG101-01",              // OPTIONAL
  "classType": "scheduled",              // REQUIRED: must be one of ClassType enum values
  "location": "Room 204",                // OPTIONAL
  "grades": "9",                         // OPTIONAL
  "subjects": "Mathematics",             // OPTIONAL
  "subjectCodes": "MATH",                // OPTIONAL
  "periods": ["1", "2"],                 // OPTIONAL
  "course": {                            // OPTIONAL
    "sourcedId": "course-123"
  },
  "school": {                            // REQUIRED
    "sourcedId": "school-123"
  },
  "session": {                           // OPTIONAL
    "sourcedId": "term-123"
  }
}
```

PUT for /classes/{sourcedId}

```typescript
{
  "title": "Algebra 101",                // REQUIRED
  "classType": "scheduled",              // REQUIRED: must be one of ClassType enum values
  "school": {                            // REQUIRED
    "sourcedId": "school-123"
  },
  "classCode": "ALG101-01",              // OPTIONAL
  "location": "Room 204",                // OPTIONAL
  "grades": "9",                         // OPTIONAL
  "subjects": "Mathematics",             // OPTIONAL
  "subjectCodes": "MATH",                // OPTIONAL
  "periods": ["1", "2"],                 // OPTIONAL
  "course": {                            // OPTIONAL
    "sourcedId": "course-123"
  },
  "session": {                           // OPTIONAL
    "sourcedId": "term-123"
  }
}
```

DELETE for /classes/{sourcedId}
The only requirement is the sourcedId in the URL path parameter.

POST for /classes/{classSourcedId}/students

```typescript
{
  "student": {                           // REQUIRED
    "sourcedId": "student-123"
  },
  "primary": true,                       // OPTIONAL: defaults to true
  "beginDate": "2024-01-15T00:00:00Z",  // OPTIONAL: ISO datetime string
  "endDate": "2024-05-30T00:00:00Z"     // OPTIONAL: ISO datetime string
}
```

POST for /classes/{classSourcedId}/teachers

```typescript
{
  "teacher": {                           // REQUIRED
    "sourcedId": "teacher-123"
  },
  "primary": true,                       // OPTIONAL: defaults to true
  "beginDate": "2024-01-15T00:00:00Z",  // OPTIONAL: ISO datetime string
  "endDate": "2024-05-30T00:00:00Z"     // OPTIONAL: ISO datetime string
}
```

POST for /courses

```typescript
{
  "sourcedId": "course-123",              // OPTIONAL: auto-generated if not provided
  "status": "active",                     // OPTIONAL: defaults to "active"
  "title": "Advanced Mathematics",        // REQUIRED
  "courseCode": "MATH301",               // OPTIONAL
  "grades": "11,12",                     // OPTIONAL: comma-separated string
  "subjects": "Mathematics,Calculus",    // OPTIONAL: comma-separated string
  "subjectCodes": "MATH,CALC",          // OPTIONAL: comma-separated string
  "org": {                              // REQUIRED
    "sourcedId": "school-123"           // REQUIRED
  }
}
```

PUT for /courses/{sourcedId}

```typescript
{
  "title": "Advanced Mathematics",        // REQUIRED
  "courseCode": "MATH301",               // OPTIONAL
  "grades": "11,12",                     // OPTIONAL: comma-separated string
  "subjects": "Mathematics,Calculus",    // OPTIONAL: comma-separated string
  "subjectCodes": "MATH,CALC",          // OPTIONAL: comma-separated string
  "org": {                              // REQUIRED
    "sourcedId": "school-123"           // REQUIRED
  }
}
```

DELETE for /courses/{sourcedId}
The only requirement is the sourcedId in the URL path parameter.

POST for /users

```typescript
{
  "sourcedId": "user-123",              // OPTIONAL: auto-generated if not provided
  "status": "active",                   // OPTIONAL: defaults to "active"
  "username": "jsmith",                 // OPTIONAL
  "enabledUser": true,                  // REQUIRED: boolean converted to string
  "givenName": "John",                  // REQUIRED
  "familyName": "Smith",                // REQUIRED
  "middleName": "Robert",               // OPTIONAL
  "email": "john.smith@school.edu",     // OPTIONAL: must be valid email if provided
  "roles": [                            // REQUIRED: array of at least one role
    {
      "roleType": "primary",            // REQUIRED: must be from RoleType enum
      "role": "student",                // REQUIRED: must be from Role enum
      "org": {                          // REQUIRED
        "sourcedId": "school-123"
      },
      "userProfile": "profile-123",     // OPTIONAL
      "beginDate": "2024-01-15",        // OPTIONAL: ISO date string
      "endDate": "2024-12-31"          // OPTIONAL: ISO date string
    }
  ],
  "grades": ["9", "10"],               // OPTIONAL: array of strings
  "password": "hashedpassword123",      // OPTIONAL
  "sms": "+1234567890",                // OPTIONAL
  "phone": "+1234567890",              // OPTIONAL
  "userIds": [                         // OPTIONAL: array of identifiers
    {
      "type": "district",
      "identifier": "12345"
    }
  ],
  "primaryOrg": {                      // OPTIONAL
    "sourcedId": "org-123"
  },
  "agents": [                          // OPTIONAL: array of references
    {
      "sourcedId": "parent-123"
    }
  ],
  "preferredFirstName": "Johnny",      // OPTIONAL
  "preferredMiddleName": "Bob",        // OPTIONAL
  "preferredLastName": "Smith",        // OPTIONAL
  "pronouns": "he/him",                // OPTIONAL
  "userProfiles": [                    // OPTIONAL
    {
      "profileId": "profile-123",
      "profileType": "academic",
      "vendorId": "vendor-123",
      "applicationId": "app-123",      // OPTIONAL
      "description": "Academic profile", // OPTIONAL
      "credentials": []                // REQUIRED: array (can be empty)
    }
  ]
}
```

PUT for /users/{sourcedId}

```typescript
{
  "username": "jsmith",                 // OPTIONAL
  "enabledUser": true,                  // REQUIRED: boolean converted to string
  "givenName": "John",                  // REQUIRED
  "familyName": "Smith",                // REQUIRED
  "middleName": "Robert",               // OPTIONAL
  "email": "john.smith@school.edu",     // OPTIONAL: must be valid email if provided
  "roles": [                            // REQUIRED: array of at least one role
    {
      "roleType": "primary",            // REQUIRED: must be from RoleType enum
      "role": "student",                // REQUIRED: must be from Role enum
      "org": {                          // REQUIRED
        "sourcedId": "school-123"
      },
      "userProfile": "profile-123",     // OPTIONAL
      "beginDate": "2024-01-15",        // OPTIONAL: ISO date string
      "endDate": "2024-12-31"          // OPTIONAL: ISO date string
    }
  ],
  "grades": ["9", "10"],               // OPTIONAL: array of strings
  "password": "hashedpassword123",      // OPTIONAL
  "sms": "+1234567890",                // OPTIONAL
  "phone": "+1234567890",              // OPTIONAL
  "preferredFirstName": "Johnny",      // OPTIONAL
  "preferredMiddleName": "Bob",        // OPTIONAL
  "preferredLastName": "Smith",        // OPTIONAL
  "pronouns": "he/him"                 // OPTIONAL
}
```

DELETE for /users/{sourcedId}
The only requirement is the sourcedId in the URL path parameter.

POST for /orgs

```typescript
{
  "sourcedId": "org-123",                // OPTIONAL: auto-generated if not provided
  "status": "active",                    // OPTIONAL: defaults to "active"
  "name": "District 100",               // REQUIRED
  "type": "district",                   // REQUIRED: must be one of OrgType enum values
  "identifier": "D100",                 // OPTIONAL
  "parent": {                           // OPTIONAL
    "sourcedId": "parent-org-123"       // REQUIRED if parent object is included
  }
}
```

PUT for /orgs/{sourcedId}

```typescript
{
  "name": "District 100",               // REQUIRED
  "type": "district",                   // REQUIRED: must be one of OrgType enum values
  "identifier": "D100",                 // OPTIONAL
  "parent": {                           // OPTIONAL
    "sourcedId": "parent-org-123"       // REQUIRED if parent object is included
  }
}
```

DELETE for /orgs/{sourcedId}
The only requirement is the sourcedId in the URL path parameter.

POST for /schools

```typescript
{
  "sourcedId": "school-123",            // OPTIONAL: auto-generated if not provided
  "status": "active",                   // OPTIONAL: defaults to "active"
  "name": "Lincoln High School",        // REQUIRED
  "type": "school",                     // REQUIRED: must be "school"
  "identifier": "LHS",                  // OPTIONAL
  "parent": {                           // OPTIONAL
    "sourcedId": "district-123"         // REQUIRED if parent object is included
  }
}
```

PUT for /schools/{sourcedId}

```typescript
{
  "name": "Lincoln High School",        // REQUIRED
  "type": "school",                     // REQUIRED: must be "school"
  "identifier": "LHS",                  // OPTIONAL
  "parent": {                           // OPTIONAL
    "sourcedId": "district-123"         // REQUIRED if parent object is included
  }
}
```

DELETE for /schools/{sourcedId}
The only requirement is the sourcedId in the URL path parameter.

POST for /demographics

```typescript
{
  "sourcedId": "demo-123",                // OPTIONAL: auto-generated if not provided
  "status": "active",                     // OPTIONAL: defaults to "active"
  "birthDate": "2006-03-15",             // OPTIONAL: YYYY-MM-DD format
  "sex": "female",                        // OPTIONAL: must be male/female/other/unspecified
  "americanIndianOrAlaskaNative": "true", // OPTIONAL: string "true"/"false"
  "asian": "false",                       // OPTIONAL: string "true"/"false"
  "blackOrAfricanAmerican": "false",      // OPTIONAL: string "true"/"false"
  "nativeHawaiianOrOtherPacificIslander": "false", // OPTIONAL: string "true"/"false"
  "white": "true",                        // OPTIONAL: string "true"/"false"
  "demographicRaceTwoOrMoreRaces": "false", // OPTIONAL: string "true"/"false"
  "hispanicOrLatinoEthnicity": "true",    // OPTIONAL: string "true"/"false"
  "countryOfBirthCode": "US",            // OPTIONAL
  "stateOfBirthAbbreviation": "CA",      // OPTIONAL
  "cityOfBirth": "Los Angeles",          // OPTIONAL
  "publicSchoolResidenceStatus": "resident" // OPTIONAL
}
```

PUT for /demographics/{sourcedId}

```typescript
{
  "birthDate": "2006-03-15",             // OPTIONAL: YYYY-MM-DD format
  "sex": "female",                        // OPTIONAL: must be male/female/other/unspecified
  "americanIndianOrAlaskaNative": "true", // OPTIONAL: string "true"/"false"
  "asian": "false",                       // OPTIONAL: string "true"/"false"
  "blackOrAfricanAmerican": "false",      // OPTIONAL: string "true"/"false"
  "nativeHawaiianOrOtherPacificIslander": "false", // OPTIONAL: string "true"/"false"
  "white": "true",                        // OPTIONAL: string "true"/"false"
  "demographicRaceTwoOrMoreRaces": "false", // OPTIONAL: string "true"/"false"
  "hispanicOrLatinoEthnicity": "true",    // OPTIONAL: string "true"/"false"
  "countryOfBirthCode": "US",            // OPTIONAL
  "stateOfBirthAbbreviation": "CA",      // OPTIONAL
  "cityOfBirth": "Los Angeles",          // OPTIONAL
  "publicSchoolResidenceStatus": "resident" // OPTIONAL
}
```

DELETE for /demographics/{sourcedId}
The only requirement is the sourcedId in the URL path parameter.

POST for /enrollments

```typescript
{
  "sourcedId": "enrollment-123",          // OPTIONAL: auto-generated if not provided
  "status": "active",                     // OPTIONAL: defaults to "active"
  "role": "student",                      // REQUIRED: must be one of student/teacher/aide/guardian/relative/proctor
  "primary": true,                        // OPTIONAL: defaults to false
  "beginDate": "2024-01-15T00:00:00Z",   // OPTIONAL: ISO datetime string
  "endDate": "2024-05-30T00:00:00Z",     // OPTIONAL: ISO datetime string
  "user": {                              // REQUIRED
    "sourcedId": "user-123"              // REQUIRED
  },
  "class": {                             // REQUIRED
    "sourcedId": "class-123"             // REQUIRED
  }
}
```

PUT for /enrollments/{sourcedId}

```typescript
{
  "sourcedId": "enrollment-123",          // REQUIRED: must match URL parameter
  "status": "active",                     // REQUIRED: must be active/tobedeleted
  "role": "student",                      // REQUIRED: must be one of student/teacher/aide/guardian/relative/proctor
  "primary": true,                        // OPTIONAL: defaults to false
  "beginDate": "2024-01-15T00:00:00Z",   // OPTIONAL: ISO datetime string
  "endDate": "2024-05-30T00:00:00Z",     // OPTIONAL: ISO datetime string
  "user": {                              // REQUIRED
    "sourcedId": "user-123"              // REQUIRED
  },
  "class": {                             // REQUIRED
    "sourcedId": "class-123"             // REQUIRED
  }
}
```

DELETE for /enrollments/{sourcedId}
The only requirement is the sourcedId in the URL path parameter.

/gradingPeriods
POST for /gradingPeriods

```typescript
{
  "sourcedId": "gp-123",                // OPTIONAL: auto-generated if not provided
  "status": "active",                   // OPTIONAL: defaults to "active"
  "title": "Q1 2024",                  // REQUIRED
  "type": "gradingPeriod",             // REQUIRED: must be "gradingPeriod"
  "startDate": "2024-01-15",           // REQUIRED: ISO date string
  "endDate": "2024-03-15",             // REQUIRED: ISO date string
  "parent": {                          // OPTIONAL
    "sourcedId": "term-123"            // REQUIRED if parent object is included
  },
  "schoolYear": "2024",                // REQUIRED
  "org": {                             // REQUIRED
    "sourcedId": "school-123"          // REQUIRED
  }
}
```

PUT for /gradingPeriods/{sourcedId}

```typescript
{
  "title": "Q1 2024",                  // REQUIRED
  "type": "gradingPeriod",             // REQUIRED: must be "gradingPeriod"
  "startDate": "2024-01-15",           // REQUIRED: ISO date string
  "endDate": "2024-03-15",             // REQUIRED: ISO date string
  "parent": {                          // OPTIONAL
    "sourcedId": "term-123"            // REQUIRED if parent object is included
  },
  "schoolYear": "2024",                // REQUIRED
  "org": {                             // REQUIRED
    "sourcedId": "school-123"          // REQUIRED
  }
}
```

DELETE for /gradingPeriods/{sourcedId}
The only requirement is the sourcedId in the URL path parameter.

Resources Service
The current OneRoster v1.2 spec for the Resources Service provides the GET method for its defined endpoints. We will layer on top POST, PUT, and DELETE methods.

Base endpoint: /ims/oneroster/resources/v1p2
OneRoster v1.2 Resources Service Extended

POST for /resources

```typescript
{
  "sourcedId": "resource-123",           // OPTIONAL: auto-generated if not provided
  "status": "active",                    // OPTIONAL: defaults to "active"
  "dateLastModified": "2024-03-21",     // REQUIRED: ISO date string
  "metadata": {                         // OPTIONAL
    "key": "value"
  },
  "title": "Biology Textbook",          // REQUIRED
  "roles": ["primary", "secondary"],    // OPTIONAL: array of role types
  "importance": "primary",              // REQUIRED: must be "primary" or "secondary"
  "vendorResourceId": "vr-123",         // OPTIONAL
  "vendorId": "vendor-123",             // OPTIONAL
  "applicationId": "app-123"            // OPTIONAL
}
```

PUT for /resources/{sourcedId}

```typescript
{
  "sourceId": "resource-123",           // REQUIRED: must match URL parameter
  "status": "active",                   // REQUIRED: must be "active" or "tobedeleted"
  "dateLastModified": "2024-03-21",    // REQUIRED: ISO date string
  "metadata": {                         // OPTIONAL
    "key": "value"
  },
  "title": "Biology Textbook",          // REQUIRED
  "roles": ["primary", "secondary"],    // OPTIONAL: array of role types
  "importance": "primary",              // REQUIRED: must be "primary" or "secondary"
  "vendorResourceId": "vr-123",         // OPTIONAL
  "vendorId": "vendor-123",             // OPTIONAL
  "applicationId": "app-123"            // OPTIONAL
}
```

DELETE for /resources/{sourcedId}
The only requirement is the sourcedId in the URL path parameter.

Gradebook Service
The Gradebook Service was updated in OneRoster v1.2, introducing POST, PUT, and DELETE endpoints, reducing the size of extended endpoints to manage gradebook data. These changes ensure a smaller scope for additional layers.

Base Endpoint: /ims/oneroster/gradebook/v1p2
OneRoster v1.2 Gradebook Service Endpoints Extended

POST /assessmentLineItems

```typescript
{
  "sourcedId": "ali-123",                // OPTIONAL: auto-generated if not provided
  "status": "active",                    // OPTIONAL: defaults to "active"
  "dateLastModified": "2024-03-21T00:00:00Z", // REQUIRED: ISO datetime string
  "metadata": {                          // OPTIONAL
    "key": "value"
  },
  "title": "Chapter 1 Test",             // REQUIRED
  "description": "End of chapter test",   // OPTIONAL
  "class": {                             // OPTIONAL
    "sourcedId": "class-123"
  },
  "parentAssessmentLineItem": {          // OPTIONAL
    "sourcedId": "parent-ali-123"
  },
  "scoreScale": {                        // OPTIONAL
    "sourcedId": "scale-123"
  },
  "resultValueMin": 0,                   // OPTIONAL: minimum score value
  "resultValueMax": 100,                 // OPTIONAL: maximum score value
  "learningObjectiveSet": [{             // OPTIONAL
    "source": "Common Core",
    "learningObjectiveIds": ["obj-123", "obj-124"]
  }]
}
```

PUT for /assessmentLineItems/{sourcedId}

```typescript
{
  "title": "Chapter 1 Test",             // REQUIRED
  "description": "End of chapter test",   // OPTIONAL
  "class": {                             // OPTIONAL
    "sourcedId": "class-123"
  },
  "parentAssessmentLineItem": {          // OPTIONAL
    "sourcedId": "parent-ali-123"
  },
  "scoreScale": {                        // OPTIONAL
    "sourcedId": "scale-123"
  },
  "resultValueMin": 0,                   // OPTIONAL: minimum score value
  "resultValueMax": 100,                 // OPTIONAL: maximum score value
  "learningObjectiveSet": [{             // OPTIONAL
    "source": "Common Core",
    "learningObjectiveIds": ["obj-123", "obj-124"]
  }]
}
```

DELETE for /assessmentLineItems/{sourcedId}
The only requirement is the sourcedId in the URL path parameter.

POST /assessmentResults

```typescript
{
  "sourcedId": "result-123",             // OPTIONAL: auto-generated if not provided
  "status": "active",                    // OPTIONAL: defaults to "active"
  "dateLastModified": "2024-03-21T00:00:00Z", // REQUIRED: ISO datetime string
  "metadata": {                          // OPTIONAL
    "key": "value"
  },
  "assessmentLineItem": {                // REQUIRED
    "sourcedId": "ali-123"
  },
  "student": {                           // REQUIRED
    "sourcedId": "student-123"
  },
  "scoreStatus": "fully_graded",         // REQUIRED: must be one of the ScoreStatus enum values
  "score": "95",                         // REQUIRED if scoreStatus is "fully_graded"
  "scoreDate": "2024-03-21T00:00:00Z",  // REQUIRED: ISO datetime string
  "comment": "Excellent work!"           // OPTIONAL
}
```

PUT for /assessmentResults/{sourcedId}

```typescript
{
  "assessmentLineItem": {                // REQUIRED
    "sourcedId": "ali-123"
  },
  "student": {                           // REQUIRED
    "sourcedId": "student-123"
  },
  "scoreStatus": "fully_graded",         // REQUIRED: must be one of the ScoreStatus enum values
  "score": "95",                         // REQUIRED if scoreStatus is "fully_graded"
  "scoreDate": "2024-03-21T00:00:00Z",  // REQUIRED: ISO datetime string
  "comment": "Excellent work!"           // OPTIONAL
}
```

DELETE for /assessmentResults/{sourcedId}
The only requirement is the sourcedId in the URL path parameter.

POST /categories

```typescript
{
  "sourcedId": "cat-123",               // OPTIONAL: auto-generated if not provided
  "status": "active",                   // OPTIONAL: defaults to "active"
  "dateLastModified": "2024-03-21T00:00:00Z", // REQUIRED: ISO datetime string
  "metadata": {                         // OPTIONAL
    "key": "value"
  },
  "title": "Homework",                  // REQUIRED
  "weight": 25.0,                      // OPTIONAL: percentage weight as number
  "class": {                           // REQUIRED
    "sourcedId": "class-123"
  },
  "parentCategory": {                   // OPTIONAL
    "sourcedId": "parent-cat-123"
  }
}
```

PUT for /categories/{sourcedId}

```typescript
{
  "title": "Homework",                  // REQUIRED
  "weight": 25.0,                      // OPTIONAL: percentage weight as number
  "class": {                           // REQUIRED
    "sourcedId": "class-123"
  },
  "parentCategory": {                   // OPTIONAL
    "sourcedId": "parent-cat-123"
  }
}
```

DELETE for /categories/{sourcedId}
The only requirement is the sourcedId in the URL path parameter.

POST /lineItems

```typescript
{
  "sourcedId": "li-123",                // OPTIONAL: auto-generated if not provided
  "status": "active",                   // OPTIONAL: defaults to "active"
  "dateLastModified": "2024-03-21T00:00:00Z", // REQUIRED: ISO datetime string
  "metadata": {                         // OPTIONAL
    "key": "value"
  },
  "title": "Midterm Essay",            // REQUIRED
  "description": "5-page essay",        // OPTIONAL
  "assignDate": "2024-03-15T00:00:00Z", // OPTIONAL: ISO datetime string
  "dueDate": "2024-03-30T00:00:00Z",   // OPTIONAL: ISO datetime string
  "class": {                           // REQUIRED
    "sourcedId": "class-123"
  },
  "category": {                        // OPTIONAL
    "sourcedId": "cat-123"
  },
  "gradingPeriod": {                   // OPTIONAL
    "sourcedId": "gp-123"
  },
  "resultValueMin": 0,                 // OPTIONAL: minimum score value
  "resultValueMax": 100,               // OPTIONAL: maximum score value
  "weight": 15.0                       // OPTIONAL: percentage weight as number
}
```

PUT for /lineItems/{sourcedId}

```typescript
{
  "title": "Midterm Essay",            // REQUIRED
  "description": "5-page essay",        // OPTIONAL
  "assignDate": "2024-03-15T00:00:00Z", // OPTIONAL: ISO datetime string
  "dueDate": "2024-03-30T00:00:00Z",   // OPTIONAL: ISO datetime string
  "class": {                           // REQUIRED
    "sourcedId": "class-123"
  },
  "category": {                        // OPTIONAL
    "sourcedId": "cat-123"
  },
  "gradingPeriod": {                   // OPTIONAL
    "sourcedId": "gp-123"
  },
  "resultValueMin": 0,                 // OPTIONAL: minimum score value
  "resultValueMax": 100,               // OPTIONAL: maximum score value
  "weight": 15.0                       // OPTIONAL: percentage weight as number
}
```

DELETE for /lineItems/{sourcedId}
The only requirement is the sourcedId in the URL path parameter.

POST /results

```typescript
{
  "title": "Midterm Essay",            // REQUIRED
  "description": "5-page essay",        // OPTIONAL
  "assignDate": "2024-03-15T00:00:00Z", // OPTIONAL: ISO datetime string
  "dueDate": "2024-03-30T00:00:00Z",   // OPTIONAL: ISO datetime string
  "class": {                           // REQUIRED
    "sourcedId": "class-123"
  },
  "category": {                        // OPTIONAL
    "sourcedId": "cat-123"
  },
  "gradingPeriod": {                   // OPTIONAL
    "sourcedId": "gp-123"
  },
  "resultValueMin": 0,                 // OPTIONAL: minimum score value
  "resultValueMax": 100,               // OPTIONAL: maximum score value
  "weight": 15.0                       // OPTIONAL: percentage weight as number
}
```

PUT for /results/{sourcedId}

```typescript
{
  "lineItem": {                        // REQUIRED
    "sourcedId": "li-123"
  },
  "student": {                         // REQUIRED
    "sourcedId": "student-123"
  },
  "scoreStatus": "fully_graded",       // REQUIRED: must be one of the ScoreStatus enum values
  "score": "95",                       // REQUIRED if scoreStatus is "fully_graded"
  "scoreDate": "2024-03-21T00:00:00Z", // REQUIRED: ISO datetime string
  "comment": "Excellent analysis!"      // OPTIONAL
}
```

DELETE for /results/{sourcedId}
The only requirement is the sourcedId in the URL path parameter.

POST /scoreScales

```typescript
{
  "sourcedId": "scale-123",             // OPTIONAL: auto-generated if not provided
  "status": "active",                   // OPTIONAL: defaults to "active"
  "dateLastModified": "2024-03-21T00:00:00Z", // REQUIRED: ISO datetime string
  "metadata": {                         // OPTIONAL
    "key": "value"
  },
  "title": "Letter Grades",             // REQUIRED
  "scaleValues": [                      // REQUIRED: array of at least one value
    {
      "numericValue": 4.0,             // REQUIRED: numeric value
      "letterGrade": "A",              // OPTIONAL: letter grade
      "description": "Excellent",       // OPTIONAL: description of grade
      "percentage": 90                  // OPTIONAL: percentage equivalent
    },
    {
      "numericValue": 3.0,
      "letterGrade": "B",
      "description": "Good",
      "percentage": 80
    }
  ]
}
```

PUT for /scoreScales/{sourcedId}

```typescript
{
  "title": "Letter Grades",             // REQUIRED
  "scaleValues": [                      // REQUIRED: array of at least one value
    {
      "numericValue": 4.0,             // REQUIRED: numeric value
      "letterGrade": "A",              // OPTIONAL: letter grade
      "description": "Excellent",       // OPTIONAL: description of grade
      "percentage": 90                  // OPTIONAL: percentage equivalent
    },
    {
      "numericValue": 3.0,
      "letterGrade": "B",
      "description": "Good",
      "percentage": 80
    }
  ]
}
```

DELETE for /scoreScales/{sourcedId}
The only requirement is the sourcedId in the URL path parameter.
