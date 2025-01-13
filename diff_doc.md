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
  "academicSession":
      {
          "sourcedId": "'$sourcedId_academicSession'",
          "title": "2023-2024 School Year",
          "startDate": "2023-08-15T00:00:00.000Z",
          "endDate": "2024-05-30T00:00:00.000Z",
          "type": "schoolYear",
          "schoolYear": "2023",
          "orgSourcedId": {
              "sourcedId": "'$org_sourcedId'"
          },
          "classSourcedId": {
              "sourcedId": "'$class_sourcedId'"
          },
          "parentSourcedId": {
              "sourcedId": "'$parent_academicSession_sourcedId'"
          }
      }
}
```

PUT for /academicSessions/{sourcedId}

```typescript
{
  "academicSession":
      {
          "sourcedId": "'$sourcedId_academicSession'",
          "title": "2024-2025 School Year",
          "startDate": "2024-08-15T00:00:00.000Z",
          "endDate": "2025-05-30T00:00:00.000Z",
          "type": "schoolYear",
          "schoolYear": "2024",
          "orgSourcedId": {
              "sourcedId": "'$org_sourcedId'"
          },
          "classSourcedId": {
              "sourcedId": "'$class_sourcedId'"
          },
          "parentSourcedId": {
              "sourcedId": "'$parent_academicSession_id'"
          }
      }
}
```

<!-- // TODO: ADD THIS TO TESTING SCRIPT, FIX BODY - DO NOT USE THIS AS AN EXAMPLE FOR DEMOGRAPHIC DATA -->

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

<!-- // TODO: ADD THIS TO TESTING SCRIPT, FIX BODY - DO NOT USE THIS AS AN EXAMPLE FOR DEMOGRAPHIC DATA -->

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

<!-- // TODO: ADD THIS TO TESTING SCRIPT, FIX BODY - DO NOT USE THIS AS AN EXAMPLE FOR DEMOGRAPHIC DATA -->

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
  "class": {
      "sourcedId": "'$sourcedId_class'",
      "status": "active",
      "metadata": {
          "test": "test"
      },
      "title": "Algebra I PUT EDIT",
      "classCode": "ALG101-01",
      "classType": "scheduled",
      "location": "Room 204",
      "grades": ["9"],
      "subjects": ["Mathematics"],
      "subjectCodes": ["MATH"],
      "periods": ["1", "2"],
      "resources": [
          {
              "sourcedId": "'$resources_sourcedId'"
          }
      ],
      "course": {
          "sourcedId": "'$course_sourcedId'"
      },
      "school": {
          "sourcedId": "'$school_sourcedId'"
      },
      "session": {
          "sourcedId": "'$term_sourcedId'"
      },
      "terms": [
          {
              "sourcedId": "'$term_sourcedId'"
          }
      ],
      "academicSessions": [
          {
              "sourcedId": "'$parent_academicSession_sourcedId'"
          }
      ]
  }
}
```

PUT for /classes/{sourcedId}

```typescript
{
  "class": {
      "sourcedId": "'$sourcedId_class'",
      "status": "active",
      "metadata": {
          "test": "test"
      },
      "title": "Algebra I PUT EDIT",
      "classCode": "ALG101-01",
      "classType": "scheduled",
      "location": "Room 204",
      "grades": ["9"],
      "subjects": ["Mathematics"],
      "subjectCodes": ["MATH"],
      "periods": ["1", "2"],
      "resources": [
          {
              "sourcedId": "'$resources_sourcedId'"
          }
      ],
      "course": {
          "sourcedId": "'$course_sourcedId'"
      },
      "school": {
          "sourcedId": "'$school_sourcedId'"
      },
      "session": {
          "sourcedId": "'$term_sourcedId'"
      },
      "terms": [
          {
              "sourcedId": "'$term_sourcedId'"
          }
      ],
      "academicSessions": [
          {
              "sourcedId": "'$parent_academicSession_sourcedId'"
          }
      ]
  }
}
```

POST for /classes/{classSourcedId}/students

```typescript
{
  "enrollment": {
      "sourcedId": "'$sourcedId_enrollment'",
      "status": "active",
      "metadata": {
          "test": "test"
      },
      "role": "student",
      "student":{
          "sourcedId": "'$student_sourcedId'"
      },
      "primary": true,
      "beginDate": "2024-08-26",
      "endDate": "2025-05-15"
  }
}
```

POST for /classes/{classSourcedId}/teachers

```typescript
{
  "enrollment": {
    "sourcedId": "'$sourcedId_enrollment'",
    "status": "active",
    "metadata": {
        "test": "test"
    },
    "role": "teacher",
    "teacher":{
        "sourcedId": "'$teacher_sourcedId'"
    },
    "primary": true,
    "beginDate": "2024-08-26",
    "endDate": "2025-05-15"
  }
}
```

POST for /courses

```typescript
{
  "course": {
    "sourcedId": "'$sourcedId_course'",
    "status": "active",
    "title": "Algebra I Course",
    "courseCode": "ALG101",
    "grades": "9",
    "subjects": "Mathematics",
    "subjectCodes": "MATH",
    "org": {
            "sourcedId": "'$school_sourcedId'"
        }
    }
}
```

PUT for /courses/{sourcedId}

```typescript
{
  "course": {
    "sourcedId": "'$sourcedId_course'",
    "status": "active",
    "title": "Algebra I Course",
    "courseCode": "ALG101",
    "grades": "9",
    "subjects": "Mathematics",
    "subjectCodes": "MATH",
    "org": {
            "sourcedId": "'$school_sourcedId'"
        }
    }
}
```

POST for /users

```typescript
{
        "user": {
      "sourcedId": "'$sourcedId_user'",
      "status": "active",
      "username": "jsmith",
      "enabledUser": true,
      "givenName": "John",
      "familyName": "Smith",
      "middleName": "Robert",
      "roles": [
        {
          "roleType": "primary",
          "role": "student",
          "org": {
            "sourcedId": "'$org_sourcedId'"
          },
          "beginDate": "2024-01-15",
          "endDate": "2024-12-31"
        }
      ],
      "primaryOrg": {
        "sourcedId": "'$org_sourcedId'"
      },
      "identifier": "jsmithy-ex-1234",
      "email": "john.smith@school.edu",
      "preferredFirstName": "Johnny",
      "grades": ["9", "10"],
      "phone": "+1234567890",
      "userProfiles": [
        {
          "profileId": "profile-123",
          "profileType": "academic",
          "vendorId": "vendor-123",
          "applicationId": "app-123",
          "description": "Academic profile",
          "credentials": []
        }
      ]
        }}
```

PUT for /users/{sourcedId}

```typescript
{
        "user": {
        "sourcedId": "'$sourcedId_user'",
        "status": "active",
        "username": "jsmith",
        "enabledUser": true,
        "givenName": "Jonathan",
        "familyName": "Smithson",
        "middleName": "Robert",
        "roles": [
            {
            "roleType": "primary",
            "role": "student",
            "org": {
                "sourcedId": "'$org_sourcedId'"
            },
            "beginDate": "2024-01-15",
            "endDate": "2024-12-31"
            }
        ],
        "primaryOrg": {
            "sourcedId": "'$org_sourcedId'"
        },
        "identifier": "jsmithy-ex-1234",
        "email": "john.smith@school.edu",
        "preferredFirstName": "Johnny",
        "grades": ["9", "10"],
        "phone": "+1234567890",
        "userProfiles": [
            {
            "profileId": "profile-123",
            "profileType": "academic",
            "vendorId": "vendor-123",
            "applicationId": "app-123",
            "description": "Academic profile",
            "credentials": []
            }
        ]
        }
  }
```

POST for /orgs

```typescript
{
  "org": {
      "sourcedId": "'$sourcedId_org'",
      "status": "active",
      "name": "District 100",
      "type": "district",
      "identifier": "D100",
      "parent": {
          "sourcedId": "'$org_sourcedId'"
      }
  }
}
```

PUT for /orgs/{sourcedId}

```typescript
{
  "org": {
      "sourcedId": "'$sourcedId_org'",
      "status": "active",
      "name": "District 100",
      "type": "district",
      "identifier": "D100",
      "parent": {
          "sourcedId": "'$org_sourcedId'"
      }
  }
}
```

POST for /schools

```typescript
{
  "org": {
      "sourcedId": "'$sourcedId_school'",
      "status": "active",
      "name": "Lincoln High School",
      "type": "school",
      "identifier": "LHS",
      "parent": {
          "sourcedId": "'$org_sourcedId'"
      }
  }
}
```

PUT for /schools/{sourcedId}

```typescript
{
  "org": {
      "sourcedId": "'$sourcedId_school'",
      "status": "active",
      "name": "Lincoln High School",
      "type": "school",
      "identifier": "LHS",
      "parent": {
          "sourcedId": "'$org_sourcedId'"
      }
  }
}
```

<!-- // TODO: ADD THIS TO TESTING SCRIPT, FIX BODY - DO NOT USE THIS AS AN EXAMPLE FOR DEMOGRAPHIC DATA -->

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

<!-- // TODO: ADD THIS TO TESTING SCRIPT, FIX BODY - DO NOT USE THIS AS AN EXAMPLE FOR DEMOGRAPHIC DATA -->

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
  "enrollment": {
      "sourcedId": "'$sourcedId_enrollment'",
      "status": "active",
      "role": "student",
      "primary": true,
      "beginDate": "2024-08-26T00:00:00Z",
      "endDate": "2025-05-15T00:00:00Z",
      "user": {
          "sourcedId": "'$student_sourcedId'"
      },
      "class": {
          "sourcedId": "'$class_sourcedId'"
      }
  }
}
```

PUT for /enrollments/{sourcedId}

```typescript
{
  "enrollment": {
      "sourcedId": "'$sourcedId_enrollment'",
      "status": "active",
      "role": "student",
      "primary": true,
      "beginDate": "2024-08-26T00:00:00Z",
      "endDate": "2025-05-15T00:00:00Z",
      "user": {
          "sourcedId": "'$student_sourcedId'"
      },
      "class": {
          "sourcedId": "'$class_sourcedId'"
      }
  }
}
```

<!-- // TODO: ADD THIS TO TESTING SCRIPT, FIX BODY - DO NOT USE THIS AS AN EXAMPLE FOR DEMOGRAPHIC DATA -->

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

<!-- // TODO: ADD THIS TO TESTING SCRIPT, FIX BODY - DO NOT USE THIS AS AN EXAMPLE FOR DEMOGRAPHIC DATA -->

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
        "sourcedId": "'$resources_sourcedId'",
        "metadata": {
            "format": "digits",
            "type": "textbook",
            "accessHistory": {
                "totalAccesses": 1250,
                "averageTimeSpent": "30 minutes",
                "lastUpdated": "2025-01-11T00:33:43.629Z"
            }
        },
        "title": "Algebra I Textbook",
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
        "sourcedId": "'$resources_sourcedId'",
        "metadata": {
            "format": "digits",
            "type": "textbook",
            "accessHistory": {
                "totalAccesses": 1250,
                "averageTimeSpent": "30 minutes",
                "lastUpdated": "2025-01-11T00:33:43.629Z"
            }
        },
        "title": "Algebra I Textbook",
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

```bash
    #########################################################
    # Assessment Line Items POST, PUT
    #########################################################
```

POST /assessmentLineItems

```typescript
{
        "assessmentLineItem": {
            "sourcedId": "'$sourcedId_assessmentLineItem'",
            "status": "active",
            "metadata": {
                "weight": "30",
                "duration": "120",
                "allowCalculator": true
            },
            "title": "Chapter 1 Test",
            "description": "End of chapter test",
            "class": {
            "sourcedId": "'$class_sourcedId'"
            },
            "parentAssessmentLineItem": {
            "sourcedId": "'$assessmentLineItem_sourcedId'"
            },
            "scoreScale": {
            "sourcedId": "'$scoreScale_sourcedId'"
            },
            "resultValueMin": 0,
            "resultValueMax": 100,
            "learningObjectiveSet": [
            {
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
            }
            ]
        }
}
```

PUT for /assessmentLineItems/{sourcedId}

```typescript
{
      "assessmentLineItem": {
        "sourcedId": "'$sourcedId_assessmentLineItem'",
        "status": "active",
        "metadata": {
            "weight": "30",
            "duration": "120",
            "allowCalculator": true
        },
        "title": "Chapter 3 Test",
        "description": "End of chapter test",
        "class": {
          "sourcedId": "'$class_sourcedId'"
        },
        "parentAssessmentLineItem": {
          "sourcedId": "'$assessmentLineItem_sourcedId'"
        },
        "scoreScale": {
          "sourcedId": "'$scoreScale_sourcedId'"
        },
        "resultValueMin": 20,
        "resultValueMax": 100,
        "learningObjectiveSet": [
          {
            "source": "Common Core",
            "learningObjectiveIds": [
              {
                "learningObjectiveId": "12345",
                "score": 88,
                "textScore": "88"
              },
              {
                "learningObjectiveId": "as12345",
                "score": 88,
                "textScore": "89"
              }
            ]
          }
        ]
      }
    }
```

```bash
    #########################################################
    # Assessment Results POST, PUT
    #########################################################
```

POST /assessmentResults

```typescript
{
  "assessmentResult": {
      "sourcedId": "'$sourcedId_assessmentResults'",
      "status": "active",
      "metadata": {
          "weight": "30",
          "duration": "120",
          "allowCalculator": true
      },
      "assessmentLineItemSourcedId": {
          "sourcedId": "'$assessmentLineItem_sourcedId'"
      },
      "studentSourcedId": {
          "sourcedId": "'$student_sourcedId'"
      },
      "score": 90,
      "textScore": "90",
      "scoreDate": "2023-12-15T00:00:00.000Z",
      "scoreScaleSourcedId": {
          "sourcedId": "'$scoreScale_sourcedId'"
      },
      "scorePercentile": 85,
      "scoreStatus": "fully graded",
      "comment": "Good work!",
      "learningObjectiveSet": [{
          "source": "Common Core",
          "learningObjectiveIds": [{
              "learningObjectiveId": "12345",
              "score": 88,
              "textScore": "88"
          }]
      }],
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
        "sourcedId": "'$sourcedId_assessmentResults'",
        "status": "active",
        "metadata": {
            "weight": "30",
            "duration": "120",
            "allowCalculator": true
        },
        "assessmentLineItemSourcedId": {
            "sourcedId": "'$assessmentLineItem_sourcedId'"
        },
        "studentSourcedId": {
            "sourcedId": "'$student_sourcedId'"
        },
        "score": 90,
        "textScore": "90",
        "scoreDate": "2023-12-15T00:00:00.000Z",
        "scoreScaleSourcedId": {
            "sourcedId": "'$scoreScale_sourcedId'"
        },
        "scorePercentile": 85,
        "scoreStatus": "fully graded",
        "comment": "Updated comment for PUT",
        "learningObjectiveSet": [{
            "source": "Common Core",
            "learningObjectiveIds": [{
                "learningObjectiveId": "12345",
                "score": 88,
                "textScore": "88"
            }]
        }],
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
            "sourcedId": "'$sourcedId_categories'",
            "status": "active",
            "metadata": {
                "type": "category",
                "weight": 0.5
            },
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
                "sourcedId": "'$sourcedId_categories'",
                "status": "active",
                "metadata": {
                    "type": "category",
                    "weight": 0.8
                },
                "title": "Participation PUT request",
                "weight": 0.8
            }
        }
```

POST /lineItems

```typescript
{
        "lineItem": {
            "sourcedId": "'$soucredId_lineItem'",
            "metadata": {
                "weight": "10",
                "isExtraCredit": false
            },
            "title": "Homework Standard 2",
            "description": "Practice problems from Chapter 1",
            "assignDate": "2023-09-05T00:00:00.000Z",
            "dueDate": "2023-09-07T00:00:00.000Z",
            "class": {
                "sourcedId": "'$class_sourcedId'"
            },
            "school": {
                "sourcedId": "'$school_sourcedId'"
            },
            "category": {
                "sourcedId": "'$category_sourcedId'"
            },
            "gradingPeriod": {
                "sourcedId": "'$gradingPeriod_sourcedId'"
            },
            "academicSession": {
                "sourcedId": "'$academicSession_sourcedId'"
            },
            "resultValueMin": 0,
            "resultValueMax": 100,
            "learningObjectiveSet": [
                {
                    "source": "State Standards",
                    "learningObjectiveIds": [
                        {
                            "learningObjectiveId": "MA.ALG.1",
                            "score": 91,
                            "textScore": "28"
                        },
                        {
                            "learningObjectiveId": "MA.ALG.2",
                            "score": 64,
                            "textScore": "28"
                        },
                        {
                            "learningObjectiveId": "MA.ALG.3",
                            "score": 87,
                            "textScore": "28"
                        }
                    ]
                }
            ]
        }
        }
```

PUT for /lineItems/{sourcedId}

```typescript
{
        "lineItem": {
            "sourcedId": "'$soucredId_lineItem'",
            "metadata": {
                "weight": "10",
                "isExtraCredit": false
            },
            "title": "Updated Homework Assignment 12",
            "description": "Practice problems from Chapter 1",
            "assignDate": "2023-09-05T00:00:00.000Z",
            "dueDate": "2023-09-07T00:00:00.000Z",
            "class": {
                "sourcedId": "'$class_sourcedId'"
            },
            "school": {
                "sourcedId": "'$school_sourcedId'"
            },
            "category": {
                "sourcedId": "'$category_sourcedId'"
            },
            "gradingPeriod": {
                "sourcedId": "'$gradingPeriod_sourcedId'"
            },
            "academicSession": {
                "sourcedId": "'$academicSession_sourcedId'"
            },
            "resultValueMin": 0,
            "resultValueMax": 100,
            "learningObjectiveSet": [
                {
                    "source": "State Standards",
                    "learningObjectiveIds": [
                        {
                            "learningObjectiveId": "MA.ALG.1",
                            "score": 91,
                            "textScore": "28"
                        },
                        {
                            "learningObjectiveId": "MA.ALG.2",
                            "score": 64,
                            "textScore": "28"
                        },
                        {
                            "learningObjectiveId": "MA.ALG.3",
                            "score": 87,
                            "textScore": "28"
                        }
                    ]
                }
            ]
        }
        }
```

POST /results

```typescript
{
        "result": {
            "sourcedId": "'$sourcedId_results'",
            "lineItem": {
                "sourcedId": "'$lineItem_sourcedId'"
            },
            "student": {
                "sourcedId": "'$student_sourcedId'"
            },
            "class": {
                "sourcedId": "'$class_sourcedId'"
            },
            "scoreStatus": "fully graded",
            "score": 85,
            "scoreDate": "2023-09-10T00:00:00.000Z",
            "comment": "Good work!",
            "learningObjectiveSet": [],
            "inProgress": false,
            "incomplete": false,
            "late": false,
            "missing": false
            }
        }
```

PUT for /results/{sourcedId}

```typescript
{
        "result": {
            "sourcedId": "'$sourcedId_results'",
            "lineItem": {
                "sourcedId": "'$lineItem_sourcedId'"
            },
            "student": {
                "sourcedId": "'$student_sourcedId'"
            },
            "class": {
                "sourcedId": "'$class_sourcedId'"
            },
            "scoreStatus": "fully graded",
            "score": 85,
            "scoreDate": "2023-09-10T00:00:00.000Z",
            "comment": "Updated - Excellent work!",
            "learningObjectiveSet": [],
            "inProgress": false,
            "incomplete": false,
            "late": false,
            "missing": false
            }
        }
```

POST /scoreScales

```typescript
{
          "scoreScale": {
              "sourcedId": "'$sourcedId_scoreScales'",
              "status": "active",
              "title": "Standard Letter Grades",
              "type": "letter",
              "class": {
                  "sourcedId": "'$class_sourcedId'"
              },
              "course": {
                  "sourcedId": "'$course_sourcedId'"
              },
              "scoreScaleValue": [
                  {
                      "itemValueLHS": "90",
                      "itemValueRHS": "100"
                  },
                  {
                      "itemValueLHS": "80",
                      "itemValueRHS": "89"
                  },
                  {
                      "itemValueLHS": "70",
                      "itemValueRHS": "79"
                  },
                  {
                      "itemValueLHS": "60",
                      "itemValueRHS": "69"
                  },
                  {
                      "itemValueLHS": "0",
                      "itemValueRHS": "59"
                  }
              ]
          }
      }
```

PUT for /scoreScales/{sourcedId}

```typescript
{
          "scoreScale": {
              "sourcedId": "'$sourcedId_scoreScales'",
              "status": "active",
              "title": "Updated Standard Letter Grades",
              "type": "letter",
              "class": {
                  "sourcedId": "'$class_sourcedId'"
              },
              "course": {
                  "sourcedId": "'$course_sourcedId'"
              },
              "scoreScaleValue": [
                  {
                      "itemValueLHS": "90",
                      "itemValueRHS": "100"
                  },
                  {
                      "itemValueLHS": "80",
                      "itemValueRHS": "89"
                  },
                  {
                      "itemValueLHS": "70",
                      "itemValueRHS": "79"
                  },
                  {
                      "itemValueLHS": "60",
                      "itemValueRHS": "69"
                  },
                  {
                      "itemValueLHS": "0",
                      "itemValueRHS": "59"
                  }
              ]
          }
      }
```

ALL DELETE METHODS ONLY REQUIRE A sourceId for /route/{sourcedId}
