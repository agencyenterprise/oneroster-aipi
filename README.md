````markdown
# API Usage with Cursor

This API was built and tested with **Cursor**. The `.cursorrules` file provided should be used when prompting Cursor. Below are the steps and examples to ensure successful API usage:

## Setup Instructions

1. Open **Composer** and switch to **Agent mode**.
2. Ensure you are using the latest Claude model (`claude-3-5-sonnet-20241022` was used during testing).
3. Add the following files to the context:
   - `.cursorrules`
   - `diff_doc.yaml`
   - `oneroster.yaml`

At this point, you can make any prompt. It's suggested to keep prompts simple and follow examples similar to those below.

---

## Prompt Examples - copy and paste into cursor if you run into similar issues

### 1. **First Prompt to Create the App**

```markdown
Please create an app that contains:

- A landing page with cards linking to other pages.
- A page that lists all `[name_of_entity]` in a table format.
- Tabs at the top for each page.
- Please ensure you are following all `.cursorrules` and the payload.
- Please ensure you are using the endpoint correctly and following the payload.

@.cursorrules @oneroster.yaml @diff_doc.yaml
```
````

---

### 2. **In Case Some Rules Are Not Followed**

```markdown
Can you double check the rules, YAML file, and diff doc to ensure you are following everything to spec?
```

---

### 3. **Debugging Errors**

```markdown
Can you double check the rules, YAML file, and diff doc to ensure you are following everything according to spec?
I am getting this error: “[add error]”
```

---

### 4. **In Case Styles Are Not Applied**

```markdown
Please check the styling imports as the styling is not showing up on the page.
```

---

### 5. **Creating Inline Editing**

```markdown
In the `[entity]` page, please make the following changes:

- When I double-click a row, make the fields for that record enter edit mode, showing a save button and a cancel button.
- Upon clicking save, the new values should be persisted to the database, and the row should return to view mode.
- Upon clicking cancel, the changes should be discarded, and the row should return to view mode.
- Please ensure you are following all `.cursorrules` and the payload.
- Please ensure you are using the endpoint correctly.
```

---

### 6. **When Relationships Are Not Showing**

```markdown
Can you double-check and make sure you are following the diff doc, cursor rules, and YAML file?
The `sourcedIds` that are required must come from actual database entities. Can you add the ability to select those entities anytime a `sourcedId` is required?
You must:

- Perform a proper GET request.
- Retrieve the IDs.
- Populate a list with titles or names.
- Allow users to select entities from a dropdown.
```

---

### 7. **When Relationships Not Defined by OneRoster Spec Need Iteration**

```markdown
I'm still not seeing `[related-entity]`. This is a GET request, and you should be referring to the YAML file, not the diff doc.
The diff doc only holds POST/PUT endpoints that are not defined by OneRoster.
```

---

### 8. **When Relationships Are Not Being Saved**

```markdown
I can see `[related-entity]` now, but when I try to save the class, I get this error: “[add error]”.
```

---

### 9. **When Progress Is Not Being Made**

```markdown
Can you analyze our conversation? What kinds of rules would you establish to avoid running into similar errors again?
I want **three rules** that any Large Language Model can understand.
```

```

```
