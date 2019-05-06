### Autofill Framework

From Android 8.0 (API Level 26) and above, Google has introduced Autofill Framework to improve its user experience. As the name suggest, the Autofill framework was introduced to Android to eliminate the repetitive task of filling the same form for different application such as username, password, address Payment Details etc.   

#### Components

Autofill Framework contains the following high level components

- _Autofill services_: Apps such as password managers that save and store the user information that can be used in views across multiple apps.

- _Autofill clients_: Apps that provide the views that need to be filled out or that hold the user's data.

- _Android system_: The OS that defines the workflow and provides the infrastructure that makes services and clients work together.

#### Workflow

Autofill Framework Consist of two API `AutofillService` and `AutofillManager`

**AutofillService**

An AutofillService is used to automatically fill the contents of the screen on behalf of a given user, It is only bound to the Android System for autofill purposes if:

1. It requires the android.permission.BIND_AUTOFILL_SERVICE permission in its manifest.
2. The user explicitly enables it using Android Settings (the `Settings#ACTION_REQUEST_SET_AUTOFILL_SERVICE` intent can be used to launch such Settings screen).

The basic autofill process is defined by the workflow below:

1. User focus an `Editable View`.
2. View calls `AutofillManager#notifyViewEntered(android.view.View)`.
3. A `ViewStructure` representing all views in the screen is created.
4. The Android System binds to the service and calls `onConnected()`.
5. The service receives the view structure through the `onFillRequest(android.service.autofill.FillRequest, android.os.CancellationSignal, android.service.autofill.FillCallback)`.
6. The service replies through `FillCallback#onSuccess(FillResponse)``.
7. The Android System calls `onDisconnected()`` and unbinds from the `AutofillService`.
8. The Android System displays an autofill UI with the options sent by the service.
9. The user picks an option.
10. The proper `views` are autofilled.

This workflow was designed to minimize the time the Android System is bound to the service; for each call, it: binds to service, waits for the reply, and unbinds right away. Furthermore, those calls are considered stateless: if the service needs to keep state between calls, it must do its own state management (keeping in mind that the service's process might be killed by the Android System when unbound; for example, if the device is running low in memory).

### References

- Autofill Framework - https://developer.android.com/guide/topics/text/autofill
- AutofillService - https://developer.android.com/reference/android/service/autofill/AutofillService
- AutofillManager - https://developer.android.com/reference/android/view/autofill/AutofillManager
