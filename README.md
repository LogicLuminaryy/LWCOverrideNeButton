# LWCOverrideNewButton

This guide outlines the steps to override the "New" button on the Account object in Salesforce and call a Lightning Web Component (LWC).

## Steps

1. **Create the Lightning Web Component (LWC)**

   Create an LWC that will be displayed when the "New" button is clicked.

   **JavaScript**

   ```javascript
   // lwc/newAccountForm/newAccountForm.js
   import { LightningElement } from 'lwc';
   import { NavigationMixin } from 'lightning/navigation';

   export default class NewAccountForm extends NavigationMixin(LightningElement) {
       handleSave() {
           // Logic to save the new Account record
           // Use lightning-record-edit-form or lightning-record-form for simplicity
       }

       handleCancel() {
           // Navigate back to the Account list view
           this[NavigationMixin.Navigate]({
               type: 'standard__objectPage',
               attributes: {
                   objectApiName: 'Account',
                   actionName: 'list'
               }
           });
       }
   }

HTML

```html
<!-- lwc/newAccountForm/newAccountForm.html -->
<template>
    <lightning-card title="New Account">
        <div class="slds-p-around_medium">
            <lightning-record-edit-form object-api-name="Account">
                <lightning-messages></lightning-messages>
                <lightning-input-field field-name="Name"></lightning-input-field>
                <lightning-input-field field-name="Industry"></lightning-input-field>
                <!-- Add more fields as needed -->

                <lightning-button class="slds-m-top_medium" variant="brand" type="submit" label="Save" onclick={handleSave}></lightning-button>
                <lightning-button class="slds-m-top_medium slds-m-left_x-small" variant="neutral" label="Cancel" onclick={handleCancel}></lightning-button>
            </lightning-record-edit-form>
        </div>
    </lightning-card>
</template>
```


**Create an Aura Component to Host the LWC
Since you can't directly set an LWC as a button override target, you need to use an Aura component to host your LWC.**


```javascript
// aura/NewAccount/NewAccountController.js
({
    // No specific controller code needed for this example
})
```
```xml
<!-- aura/NewAccount/NewAccount.cmp -->
<aura:component implements="force:appHostable,flexipage:availableForAllPageTypes,force:hasRecordId,lightning:hasPageReference">
    <c:newAccountForm></c:newAccountForm>
</aura:component>
Override the "New" Button on the Account Object
```
**Go to Setup.**

Navigate to Object Manager and select Account.

Select Buttons, Links, and Actions.

Find the New button and click Edit.

Change the behavior to Lightning Component.

Select the Aura component you created (NewAccount).

Deploy Your Components

Make sure you deploy both your LWC and Aura components to your Salesforce org. You can do this using Salesforce CLI or through your preferred IDE.

**Summary**

Create an LWC to handle the new Account form.

Create an Aura component to host the LWC.

Override the "New" button on the Account object to point to the Aura component.

Deploy the components to your Salesforce org.

Once these steps are completed, clicking the "New" button on the Account object should display your custom LWC form.
