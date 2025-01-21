# API Usage with Cursor

This API was built and tested with **Cursor**. The `.cursorrules` file provided should be used when prompting Cursor. Below are the steps and examples to ensure successful API usage:

## Setup Instructions (Speciic if you are testing in a Cursor environment)

1. Open **Composer** and switch to **Agent mode** 
2. Ensure you are using the latest Claude model (`claude-3-5-sonnet-20241022` was used during testing).
3. Add the following files to the context:
   - `.cursorrules`
   - `rostersservice.yaml`
   - `gradebookservice.yaml`
   - `resourcesservice.yaml`
   - `extensions.yaml`

Suggested Prompts:

- Please build me an app that allows me to view student data.
- Please build me a sis that allows me to view and edit classes and teachers.
- Please build me an app that allows me to view, edit, and delete classes and teachers.
