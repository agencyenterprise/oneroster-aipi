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

POST for /users

```typescript
{
  "sourcedId": "user-123",              // OPTIONAL: auto-generated if not provided
  "metadata": {},                       // OPTIONAL
  "status": "active",                   // REQUIRED: defaults to 'active'
  "userMasterIdentifier",               // OPTIONAL
  "username": "jsmith",                 // OPTIONAL
    "userIds": [                         // OPTIONAL: array of identifiers
    {
      "type": "district",
      "identifier": "12345"
    }
  ],
  "enabledUser": true,                  // REQUIRED: boolean converted to string
  "givenName": "John",                  // REQUIRED
  "familyName": "Smith",                // REQUIRED
  "middleName": "Robert",               // OPTIONAL
  "roles": [                            // REQUIRED: array, defaults to []
    {
      "roleType": "primary",            // REQUIRED: must be from RoleType enum
      "role": "student",                // REQUIRED: must be from Role enum
      "org": {                          // REQUIRED
        "sourcedId": "school-123"       // REQUIRED
      },
      "userProfile": "profile-123",     // OPTIONAL
      "beginDate": "2024-01-15",        // REQUIRED but NULLABLE: ISO date string
      "endDate": "2024-12-31"           // REQUIRED but NULLABLE: ISO date string
    }
  ],
  "primaryOrg": {                       // OPTIONAL
    "sourcedId": "org-123"              // REQUIRED if primaryOrg is included in POST request
  },
  "identifier": "jsmithy-ex-1234",      // OPTIONAL string
  "email": "john.smith@school.edu",     // OPTIONAL: must be valid email if provided
  "agents": [                           // OPTIONAL: array of references
    {
      "sourcedId": "parent-123"        // REQUIRED if agents are included
    }
  ],
  "resources": [                       // OPTIONAL
    "sourcedId": "12345"               // REQUIRED if 'resources' are included in POST request
  ]
  "preferredFirstName": "Johnny",      // OPTIONAL
  "preferredMiddleName": "Bob",        // OPTIONAL
  "preferredLastName": "Smith",        // OPTIONAL
  "pronouns": "he/him",                // OPTIONAL
  "grades": ["9", "10"],               // OPTIONAL: array of strings
  "password": "hashedpassword123",      // OPTIONAL
  "sms": "+1234567890",                // OPTIONAL
  "phone": "+1234567890",              // OPTIONAL
  "userProfiles": [                    // OPTIONAL if included there are REQUIRED key values pairs designated below
    {
      "profileId": "profile-123",        // REQUIRED only required if userProfiles is included in POST request
      "profileType": "academic",         // REQUIRED only required if userProfiles is included in POST request
      "vendorId": "vendor-123",          // REQUIRED only required if userProfiles is included in POST request
      "applicationId": "app-123",        // REQUIRED only required if userProfiles is included in POST request
      "description": "Academic profile", // REQUIRED only required if userProfiles is included in POST request
      "credentials": []                  // REQUIRED: array (can be empty)  only required if userProfiles is included in POST request
    }
  ]
}
```

PUT for /users/{sourcedId}

```typescript
{
  "status": "active",                   // REQUIRED: defaults to 'active'
  "userMasterIdentifier",               // OPTIONAL
  "username": "jsmith",                 // OPTIONAL
    "userIds": [                         // OPTIONAL: array of identifiers
    {
      "type": "district",
      "identifier": "12345"
    }
  ],
  "enabledUser": true,                  // REQUIRED: boolean converted to string
  "givenName": "John",                  // REQUIRED
  "familyName": "Smith",                // REQUIRED
  "middleName": "Robert",               // OPTIONAL
  "roles": [                            // REQUIRED: array, defaults to []
    {
      "roleType": "primary",            // REQUIRED: must be from RoleType enum
      "role": "student",                // REQUIRED: must be from Role enum
      "org": {                          // REQUIRED
        "sourcedId": "school-123"       // REQUIRED
      },
      "userProfile": "profile-123",     // OPTIONAL
      "beginDate": "2024-01-15",        // REQUIRED but NULLABLE: ISO date string
      "endDate": "2024-12-31"          // REQUIRED but NULLABLE: ISO date string
    }
  ],
  "primaryOrg": {                      // OPTIONAL
    "sourcedId": "org-123"             // REQUIRED if primaryOrg is included in POST request
  },
  "identifier": "jsmithy-ex-1234",     // OPTIONAL string
  "email": "john.smith@school.edu",     // OPTIONAL: must be valid email if provided
  "agents": [                          // OPTIONAL: array of references
    {
      "sourcedId": "parent-123"        // REQUIRED if agents are included
    }
  ],
  "resources": [                       // OPTIONAL
    "sourcedId": "12345"               // REQUIRED if 'resources' are included in POST request
  ]
  "preferredFirstName": "Johnny",      // OPTIONAL
  "preferredMiddleName": "Bob",        // OPTIONAL
  "preferredLastName": "Smith",        // OPTIONAL
  "pronouns": "he/him",                // OPTIONAL
  "grades": ["9", "10"],               // OPTIONAL: array of strings
  "password": "hashedpassword123",      // OPTIONAL
  "sms": "+1234567890",                // OPTIONAL
  "phone": "+1234567890",              // OPTIONAL
  "userProfiles": [                    // OPTIONAL if included there are REQUIRED key values pairs designated below
    {
      "profileId": "profile-123",        // REQUIRED only required if userProfiles is included in POST request
      "profileType": "academic",         // REQUIRED only required if userProfiles is included in POST request
      "vendorId": "vendor-123",          // REQUIRED only required if userProfiles is included in POST request
      "applicationId": "app-123",        // REQUIRED only required if userProfiles is included in POST request
      "description": "Academic profile", // REQUIRED only required if userProfiles is included in POST request
      "credentials": []                  // REQUIRED: array (can be empty)  only required if userProfiles is included in POST request
    }
  ]
}
```

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

Resources Service
The current OneRoster v1.2 spec for the Resources Service provides the GET method for its defined endpoints. We will layer on top POST, PUT, and DELETE methods.

Base endpoint: /ims/oneroster/resources/v1p2
OneRoster v1.2 Resources Service Extended

POST for /resources

```typescript
{
  "resource":
      {
          "sourcedId": "'$resources_id'",
          "metadata": {
              "format": "digits",
              "type": "textbook",
              "accessHistory": {
                  "totalAccesses": 1250,
                  "averageTimeSpent": "30 minutes",
                  "lastUpdated": "2025-01-11T00:33:43.629Z"
              }
          },
          "title": "'$resource_title'",
          "roles": [
              "primary"
          ],
          "importance": "primary",
          "vendorResourceId": "ee42cb40-35b2-4092-8ae8-c5e6f07ac785",
          "vendorId": "pearson_ed",
          "applicationId": "digital_textbook"
      }
}
```

<!-- TODO - PUT REQUIRED UNDER EACH OF THESE -->

PUT for /resources/{sourcedId}

```typescript
{
  "resource":
      {
          "sourcedId": "'$resources_id'",
          "metadata": {
              "format": "digits",
              "type": "textbook",
              "accessHistory": {
                  "totalAccesses": 1250,
                  "averageTimeSpent": "30 minutes",
                  "lastUpdated": "2025-01-11T00:33:43.629Z"
              }
          },
          "title": "'$resource_title'",
          "roles": [
              "primary"
          ],
          "importance": "primary",
          "vendorResourceId": "ee42cb40-35b2-4092-8ae8-c5e6f07ac785",
          "vendorId": "pearson_ed",
          "applicationId": "digital_textbook"
      }
}
```

<!-- TODO - PUT REQUIRED UNDER EACH OF THESE -->

Gradebook Service
The Gradebook Service was updated in OneRoster v1.2, introducing POST, PUT, and DELETE endpoints, reducing the size of extended endpoints to manage gradebook data. These changes ensure a smaller scope for additional layers.

Base Endpoint: /ims/oneroster/gradebook/v1p2
OneRoster v1.2 Gradebook Service Endpoints Extended

POST /assessmentLineItems

```typescript
{
  "sourcedId": "ali-123",                // OPTIONAL: auto-generated if not provided
  "status": "active",                    // OPTIONAL: defaults to "active"
  "dateLastModified": "2024-03-21T00:00:00Z", // OPTIONAL auto generated by server
  "metadata": {                          // OPTIONAL
    "key": "value"
  },
  "title": "Chapter 1 Test",             // REQUIRED
  "description": "End of chapter test",   // OPTIONAL
  "class": {                             // OPTIONAL but should be included for relations
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
    "learningObjectiveIds": [
      {
        "learningObjectiveId": "12345",
        "score": 89,
        "textScore": "89"
      },
      {
        "learningObjectiveId": "as12345",
        "score": 89,
        "textScore": "89"
      }
      ]
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

POST /assessmentResults

```typescript
{
    "assessmentResult": {
        "assessmentLineItem": {               // REQUIRED
            "sourcedId": "74f93ce8-65d8-4ab2-81ce-9eaaf07ea129",
        },
        "student": {                          // REQUIRED
            "sourcedId": "099f9d21-a73d-4c87-9a21-c4bf722316c9",
        },
        "score": 85,
        "scoreDate": "2023-12-15T00:00:00.000Z",      // REQUIRED
        "scoreStatus": "fully graded",                // REQUIRED
        "comment": "Good understanding of core concepts",
        "learningObjectiveSet": [],
        "inProgress": true,
        "incomplete": false,
        "late": false,
        "missing": false
    }
}
```

PUT for /assessmentResults/{sourcedId}

```typescript
{
    "assessmentResult": {
        "assessmentLineItem": {               // REQUIRED
            "sourcedId": "74f93ce8-65d8-4ab2-81ce-9eaaf07ea129",
        },
        "student": {                          // REQUIRED
            "sourcedId": "099f9d21-a73d-4c87-9a21-c4bf722316c9",
        },
        "score": 85,
        "scoreDate": "2023-12-15T00:00:00.000Z",      // REQUIRED
        "scoreStatus": "fully graded",                // REQUIRED
        "comment": "Good understanding of core concepts",
        "learningObjectiveSet": [],
        "inProgress": true,
        "incomplete": false,
        "late": false,
        "missing": false
    }
}
```

POST /categories

```typescript
{
    "category":
        {
            "title": "Participation",
            "weight": 0.5
        }
}
```

PUT for /categories/{sourcedId}

```typescript
{
    "category":
        {
            "title": "Participation",
            "weight": 0.5
        }
}
```

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

ALL DELETE METHODS ONLY REQUIRE A sourceId for /route/{sourcedId}
