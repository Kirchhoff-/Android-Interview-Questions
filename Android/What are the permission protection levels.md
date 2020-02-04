## Protection levels in permission

**Normal** - The default value. A lower-risk permission that gives requesting applications access to isolated application-level features, with minimal risk to other applications, the system, or the user. The system automatically grants this type of permission to a requesting application at installation, without asking for the user's explicit approval (though the user always has the option to review these permissions before installing).

**Dangerous** - A higher-risk permission that would give a requesting application access to private user data or control over the device that can negatively impact the user. Because this type of permission introduces potential risk, the system may not automatically grant it to the requesting application. For example, any dangerous permissions requested by an application may be displayed to the user and require confirmation before proceeding, or some other approach may be taken to avoid the user automatically allowing the use of such facilities.

**Signature** - A permission that the system grants only if the requesting application is signed with the same certificate as the application that declared the permission. If the certificates match, the system automatically grants the permission without notifying the user or asking for the user's explicit approval.

**SignatureOrSystem** - Old synonym for **"signature|privileged"**. Deprecated in API level 23.

A permission that the system grants only to applications that are in a dedicated folder on the Android system image or that are signed with the same certificate as the application that declared the permission. Avoid using this option, as the signature protection level should be sufficient for most needs and works regardless of exactly where apps are installed. The "signatureOrSystem" permission is used for certain special situations where multiple vendors have applications built into a system image and need to share specific features explicitly because they are being built together.

## Links
https://developer.android.com/guide/topics/manifest/permission-element
