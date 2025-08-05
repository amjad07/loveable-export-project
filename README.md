Confirmation Email Function Called Twice
File:
src/components/LeadCaptureForm.tsx
Severity: Medium Status: ✅ Fixed
Problem
The confirmation email function (send-confirmation) was being called twice during form submission, causing:
* Redundant API calls
* Increased load and risk of 500 Internal Server Errors
* Possible double emails to leads
Root Cause
A copy-paste error caused two identical supabase.functions.invoke() calls to be made back-to-back in the handleSubmit method.
Fix
Removed the duplicated call and retained a single invocation wrapped in a try/catch block:
tsx
CopyEdit
const { error: emailError } = await supabase.functions.invoke('send-confirmation', {
  body: {
    name: formData.name,
    email: formData.email,
    industry: formData.industry,
  },
});
Impact
* ✅ Reduced server load
* ✅ Eliminated 500 errors from duplicate invocations
* ✅ Prevented sending multiple confirmation emails per submission
* ✅ Cleaner and more maintainable form logic


Missing Loader on Form Submission
File:
src/components/LeadCaptureForm.tsx
Severity: Low
Status: ✅ Fixed

Problem
When clicking Get Early Access, there was no indication that the form was being submitted. This led users to think the button was unresponsive, possibly causing:

Multiple rapid clicks

Duplicate submissions

User confusion

Root Cause
The form lacked a loading state during async confirmation email sending.

Fix
Added a loading state using useState, displayed a loader while supabase.functions.invoke runs, and disabled form inputs during the async operation.

const [loading, setLoading] = useState(false);
{loading ? <p>Sending confirmation...</p> : null}
Impact
✅ Improved UX with feedback on submission

✅ Prevented repeated form submissions

✅ Increased user confidence
