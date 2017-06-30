## SCHEMA EXAMPLES

1. [BASIC QUESTIONS TYPES](#basic-questions-types)
2. [ADVANCED QUESTIONS TYPES](#advanced-questions-types)
3. [STYLING](#styling)
4. [VALIDATION](#validation)
5. [ADDITIONAL PROPS](#additional-props)
6. [TIPS AND TRICKS](#tips-and-tricks)

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

### ADVANCED QUESTIONS TYPES

#### FileInput
This will show a "Import File" button and open file reading dialog when clicked. 

```json
{
  "questionId": "sshKey",
  "input": {
    "type": "FileInput"
  }
}
```
After selecting file it will read the file name, size and it's content as text into this question value.
This is different because usually the questions value is string or boolean but in this case it's an object:
```javascript
{
  name: "someFile.txt",
  size: 1234,
  content: "some content here"
}
```
Name and size will be shown after reading the file. There's also a X button that will remove the file.

#### ArrayOfElements

This field allows to add multiple groups of questions. 

```json
{
  "questionId": "proxies",
  "question": "Proxies",
  "input": {
    "type": "ArrayOfElements",
    "addText": "Add Proxy",
    "options": [
      {
        "template": [
          {
            "questionId": "address",
            "question": "IP",
            "input": {
              "type": "Input"
            }
          },
          {
            "questionId": "port",
            "question": "Port",
            "input": {
              "type": "Input"
            }
          }
        ]
      }
    ]
  }
}
```

You have to provide "addText" prop, that will be shown next to "+" button for adding new fields.

You also have to provide a template - array of questions. This array will be added as a group each time you click the "add" button. You also can remove each group by clicking (X) button in the top right corner.

If you want to get the value of question which is inside the array yo have to use "object-path" notation. 
For example if there are 2 array elements added with the example above then getting the value of port field in the second element would be "proxies.1.port".

#### Conditional Questions
Each question can render other questions below it, based on it's value. In order to do that you need to provide "conditionalQuestions" property inside "input" property of question. 

This "conditionalQuestions" is an array of questions that will be shown on certain value. Example:

```json
{
  "questionId": "overrideDut",
  "question": "Override DUT IP (v4/v6)",
  "input": {
    "type": "Toggle",
    "default": false,
    "options": [
      {
        "value": true,
        "conditionalQuestions": [
          {
            "questionId": "overrideDutAddress",
            "question": "Address",
            "input": {
              "type": "Input"
            }
          }
        ]
      }
    ]
  }
}
```
In this example we will show additional Input field when Toggle value will change to `true`. Of course we could show some other field on `false` as well - we would need to add another element to `options` array with `value` as false.

This can be used not only with Toggle type but with ButtonsGroup or Dropdown as well. The idea is the same - you provide the options array with values corresponding to parent question values.

### STYLING


