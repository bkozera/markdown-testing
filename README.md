## SCHEMA EXAMPLES

### BASIC QUESTIONS TYPES

```json
{
  "questionId": "name",
  "question": "Profile Name",
  "input": {
    "type": "Input"
  }
}
```

Each question is an object. It requires `questionId` which has to be unique accross the whole schema and `input` property with `type` of question.

`question` is a label that will be shown next to input. It's optional but it will still leave the space in the form (this of it as `visible: none` not `display: none`). 
There are ways to remove the label.

Available types are:
#### Input
Basic input file. by default it has type of "text" but it's possible to overwrite this. We currently only do it when using "password" type:
```json
{
  "questionId": "password",
  "question": "Password",
  "input": {
    "type": "Input",
    "props": {
      "type": "password"
    }
  }
}
```
We don't use other types. Even if the the value should be a number we still use text type and convert to number before sending to API.
If you need type `file` use `FileInput` component

#### TextArea
This is a basic text area field, similar to input, just bigger.
```json
{
  "questionId": "additionalHeaders",
  "question": "Additional Headers",
  "input": {
    "type": "TextArea"
  }
}
```
#### Description
This a textarea field with additional character counter in the top right corner.
```
{
  "questionId": "description",
  "question": "Description",
  "input": {
    "type": "Description",
    "props": {
      "placeholder": "Describe the goal(s) of this profile for future reference"
    }
  }
}
````
#### Toggle
Simple toggle button with two boolean values. You usually want to provide default value and other values (which is the opposite boolean)
```json
{
  "questionId": "ssl",
  "question": "Enable SSL",
  "input": {
    "type": "Toggle",
    "default": false,
    "options": [
      {
        "value": true
      }
    ]
  }
}
```
#### ButtonsGroup
Groups of button from wich only one option can be selected. Yo uhave to provide the default value and all of the buttons options, each with label and value.
```json
{
  "questionId": "authentication",
  "question": "Authentication",
  "input": {
    "type": "ButtonsGroup",
    "default": "cookie",
    "props": {
      "buttons": [
        {
          "value": "cookie",
          "label": "Cookie"
        },
        {
          "value": "token",
          "label": "Token"
        },
        {
          "value": "http",
          "label": "HTTP Authentication"
        }
      ]
    }
  }
}
```
#### Dropdown
Typical dropdown, equivalent of select element. You have to provide the default value and all of the options, each with value and label.
```json
{
  "questionId": "httpMethod",
  "question": "HTTP Method",
  "input": {
    "type": "Dropdown",
    "default": "GET",
    "options": [
      {
      "value": "GET",
      "label": "GET"
      },
      {
      "value": "POST",
      "label": "POST"
      }
  ]
  }
}
```
