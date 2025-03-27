# HeatSuite Watch App - Ecological Momentary Assessments

## What are Ecological Momentary Assessments (aka EMA)?

Ecological Momentary Assessment (EMA) is a research method that involves collecting data about an individuals' behaviors, experiences, emotions, or physiological states in real time and in their natural environments. This method enables the collection of *"right here, right now"* data whereby users respond to questions directly on the watch.

## What questions can I ask?

Short answer, anything! As HeatSuite was designed to be a researcher first tool, it does not come with pre-defined questions. You are in full control of what questions you want to ask, and their design - within the limits of the software. In brief, the best questions are short with simple responses (one/two words per option).

## How do I add EMA questions? 

Adding EMA to the watch app requires you to simply upload a properly formatted heatsuite.survey.json file to the watch. This is done during the HeatSuite app installation on the Bangle.js2. If you want to add in additional programmatic "nudging", you will need to include the survey task in your heatsuite.tasks.json file ([how do I do that?](watchapp-tasks.md)).

## Formatting heatsuite.survey.json

Below is an example of a heatsuite.survey.json file with one question, with two available languages that you can use to build your EMA questions from:

```json
{
"supported":{
    "en_GB":"English (GB)",
    "fr_CA":"Francais (CA)"
    },
"questions":[{
    "key":"comfort",
    "text": {
        "en_GB":"Thermal comfort?",
        "fr_CA":"Confort thermique?"
        },
    "tod":[[0,2359]],
    "oncePerDay": true,
    "orderFix":false,
    "options": [{
        "text":{
            "en_GB":"Comfortable",
            "fr_CA":"Confortable"
            },
        "value":0,
        "color":"#ffffff",
        "btnColor":"#38ed35"
        },{
        "text":{
            "en_GB":"Uncomfortable",
            "fr_CA": "Inconfortable"
            },
        "value":1,
        "color":"#ffffff",
        "btnColor":"#ff0019"
        }]
    }]
}
```
And this will present us with this question on the watch:


Lets break it down:
```json
"supported":{
    "en_GB":"English (GB)",
    "fr_CA":"Francais (CA)"
    }
```
`supported` is object `{}` which contains all supported languages for your EMA. You must include all supported languages here as `key:value` with the key being the identifier (e.g. `en_GB`) and the value being the descriptor (e.g. `English (GB)`). The user will be able to select their prefered language within `Settings->Apps->HeatSuite`. HeatSuite recommends following the language convention used within [BangleApps locales.js](https://github.com/espruino/BangleApps/blob/master/apps/locale/locales.js).
```json
"questions":[]
```
`questions` will contain an array `[]` of objects `{}`. Within each object is the details and settings of your question. Below is what a question object would look like:

```json
{
    "key":"comfort",
    "text": {
        "en_GB":"Thermal comfort?",
        "fr_CA":"Confort thermique?"
        },
    "tod":[[0,2359]],
    "oncePerDay": true,
    "orderFix":false,
    "options": [{
        "text":{
            "en_GB":"Comfortable",
            "fr_CA":"Confortable"
            },
        "value":0,
        "color":"#ffffff",
        "btnColor":"#38ed35"
        },{
        "text":{
            "en_GB":"Uncomfortable",
            "fr_CA": "Inconfortable"
            },
        "value":1,
        "color":"#ffffff",
        "btnColor":"#ff0019"
        }],
}
```
* `key`: The definition/classification you want to give this question
* `text`: an object `{}` containing the question/prompt with keys corresponding to all supported languages and values containing the text to be presented.
* `tod`(optional): an array `[]` of arrays `[]` containing possible restrictions of the question depending on time of day (in 24h clock). Within this example, The question may be presented at any time of day (`[0,2359]`). However, if you want restrict the question to only be visible in the morning, you would adjust to `[0,1159]`. If you only want questions to be restricted to specific times of day, you can add multiple time windows in the array like this: `"tod":[[0,1159],[1400,1800]]`. In this example, you will only see the question between 00:00 & 11:59, or 14:00 & 18:00.  
* `oncePerDay`(optional): a boolean value (`true` or `false`) to restrict the question to only appear once a day.
* `orderFix` (optional): a boolean value (`true` or `false`) to fix the order of this question if you have selected question randomization in HeatSuite settings.
* `options`: an array `[]` of objects `{}` containing details for each option to present for the user to select. Within each object, you have the following options:
    * `text`: and object `{}` containing your language specific (key) option text (value).
    * `value`: the numerical value you wish for response coding.
    * `color`: The color of the option text.
    * `btnColor`: The color of the option background.

