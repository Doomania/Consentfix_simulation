# ConsentFix SOC User Journey Simulator

An interactive, browser-based training lab that helps Security Operations Centre analysts understand the **ConsentFix** OAuth phishing technique from both the user and defender perspectives.

The simulator presents realistic but nonfunctional user-facing screens, including a verification lure, Microsoft-style sign-in experience, MFA prompt, localhost callback error, callback handoff, and post-handoff state. At every stage, it explains what may be happening behind the scenes and highlights relevant Microsoft Defender XDR and Microsoft Entra investigation pivots.

> **Safety notice**
>
> This project is an educational, defensive simulation. It does not authenticate users, collect credentials, create OAuth authorization requests, contact Microsoft identity endpoints, capture authorization codes, redeem tokens, or replay sessions. All displayed values are synthetic placeholders.

## Purpose

ConsentFix can be difficult for a SOC to investigate because parts of the user journey may involve genuine Microsoft authentication pages and successfully completed authentication controls. The decisive malicious action may occur when the user is manipulated into transferring authorization material from a localhost callback to an untrusted page.

This lab is designed to help analysts:

- Understand the attack chain without operating a live attack
- Recognize what the affected user may remember or report
- Ask targeted questions without requesting sensitive authentication material
- Identify useful Microsoft Defender XDR and Microsoft Entra telemetry
- Distinguish exposure from suspected token compromise and confirmed impact
- Avoid overclaiming compromise based only on MFA success, an IP change, or a suspicious page visit
- Apply an evidence-led containment and escalation process

## Features

- Six-stage interactive ConsentFix walkthrough
- Realistic, nonfunctional user-page simulations
- Persistent training watermark on identity-style screens
- Display-only fields with no credential collection
- Stage-specific user interview questions
- Microsoft Defender XDR hunting pivots
- Microsoft Entra investigation checks
- Progressive attack timeline
- Confirmation and confidence model
- Clickable containment checklist
- Keyboard navigation using the left and right arrow keys
- Responsive, dependency-free single-file HTML design
- No external JavaScript, fonts, images, APIs, or tracking

## Simulated Attack Stages

### 1. Lure Page

Shows a fake verification or CAPTCHA-style page that may be reached through search results, advertisements, email, chat, redirects, or a compromised website.

SOC focus:

- Original URL and referral path
- Browser and network evidence
- User activity immediately before the page appeared
- Whether Microsoft identity infrastructure had been contacted yet

### 2. Microsoft Sign-In

Shows a Microsoft-style sign-in page to demonstrate that the authentication page involved in the flow may appear legitimate.

SOC focus:

- Application and client ID
- Resource being accessed
- Interactive sign-in context
- Source IP, device, browser, session, and Conditional Access results
- Whether the application and workflow were expected by the user

### 3. MFA or Existing Session

Shows an illustrative number-matching prompt and explains that authentication may be completed normally or may already be satisfied by an existing session.

SOC focus:

- Authentication method and requirement
- Whether the prompt was expected
- Actual Conditional Access evaluation
- Relationship to the suspicious browser workflow

### 4. Localhost Callback

Shows a browser connection-error page with a redacted localhost callback placeholder.

SOC focus:

- Whether the user saw `localhost` or `127.0.0.1`
- Whether the user was told to copy, paste, or drag the address
- Browser and endpoint evidence
- Whether an expected local CLI or native client was running

### 5. Callback Handoff

Shows a nonfunctional page asking the user to complete verification with a redacted placeholder.

SOC focus:

- Whether the user submitted the callback
- Approximate submission time
- Destination domain
- Outbound browser activity after the localhost event
- Urgent identity containment when handoff is confirmed

### 6. Session Use and Cloud Impact

Shows that the user may notice no visible change after the handoff.

SOC focus:

- Subsequent interactive and non-interactive identity activity
- Unrecognized device, application, IP, ASN, or session context
- Microsoft Graph and cloud application activity
- Mailbox rules, forwarding, sent messages, file access, sharing, directory changes, and application changes
- Evidence required to establish actual impact

## Investigation Model

The simulator separates findings into three broad confidence levels.

### Exposure

The user visited a suspicious lure or initiated an unexpected authorization flow.

Exposure alone does not prove that authorization material was transferred or that a resulting session was used.

### Suspected Token Compromise

The user confirms that they copied, pasted, dragged, or submitted the localhost callback value to an untrusted page, or the available evidence strongly supports that handoff.

This state should trigger urgent containment under the organization's approved identity-response process.

### Confirmed Activity or Impact

Identity, audit, or workload evidence links the resulting session to actions taken against cloud resources.

Analysts should document only the resources and actions supported by evidence. Do not infer mailbox access, data theft, persistence, or privilege change solely from suspected token compromise.

## Telemetry Highlighted in the Lab

The simulator references investigation sources that may include:

### Microsoft Defender XDR

- `EntraIdSignInEvents`
- `IdentityLogonEvents`
- `DeviceNetworkEvents`
- `DeviceProcessEvents`
- `DeviceEvents`
- `DeviceInfo`
- `AlertInfo`
- `AlertEvidence`
- `CloudAppEvents`
- `EmailEvents`
- `EmailPostDeliveryEvents`
- `EmailUrlInfo`
- `UrlClickEvents`
- `MicrosoftGraphActivityLogs`

### Microsoft Entra

- Interactive sign-in logs
- Non-interactive sign-in logs
- Authentication details
- Conditional Access evaluation
- Identity Protection risk detections
- Audit logs
- Enterprise application permissions and assignments
- Application and service-principal sign-in activity
- Correlation, request, and session identifiers where available

> Telemetry availability and field population vary by tenant configuration, licensing, ingestion, and retention. Validate all table and field names in your own environment.

## Safe Evidence Handling

Do not ask an affected user to paste any of the following into Teams, email, ServiceNow, GitHub issues, or other general collaboration systems:

- Localhost callback URLs
- OAuth authorization codes
- Access or refresh tokens
- Session cookies
- Passwords
- MFA codes
- Client secrets
- Other live authentication material

Record that the value existed, preserve relevant evidence through approved security channels, and redact sensitive values from screenshots and tickets.

## Running the Simulator

No build process or package installation is required.

1. Download or clone the repository.
2. Open `ConsentFix_SOC_User_Page_Simulation.html` in a modern web browser.
3. Use **Next**, **Back**, the stage buttons, or the keyboard arrow keys to move through the scenario.
4. Use the lower tabs to review telemetry, timeline, confirmation logic, and containment actions.

You can also host the file as a static page using GitHub Pages or another approved internal static-web platform.

## Suggested Repository Structure

```text
consentfix-soc-simulator/
├── ConsentFix_SOC_User_Page_Simulation.html
├── README.md
├── LICENSE
└── screenshots/
    ├── lure-page.png
    ├── microsoft-sign-in-simulation.png
    ├── localhost-callback.png
    └── soc-telemetry.png
```

The `screenshots` directory is optional but useful for the GitHub project page and internal training material.

## Security Design

The simulator intentionally excludes:

- Functional login forms
- Password input fields
- Live OAuth client identifiers or reusable authorization parameters
- Live redirect URIs
- Token endpoints
- Network submission logic
- Credential or token storage
- Browser storage of sensitive values
- External analytics or tracking
- Code redemption or token replay functionality

The Microsoft-style pages are training visuals only. They are clearly marked as simulations and do not accept input.

## Customization

The project is a single HTML file. You can customize:

- Training text
- Incident-response terminology
- SOC interview questions
- XDR and Entra telemetry references
- Containment actions
- Colors and layout
- Organization-specific escalation guidance

When customizing the project, keep all authentication values synthetic and retain the visible simulation warning.

## Limitations

- This is not a detection rule or automated investigation tool.
- It does not query Microsoft Defender XDR or Microsoft Entra.
- It does not establish whether a real incident is malicious.
- It does not reproduce every possible ConsentFix variation.
- It does not guarantee that all referenced telemetry is enabled in a given tenant.
- It should not replace an organization's approved incident-response or identity-containment playbook.

## Intended Audience

- SOC analysts
- Incident responders
- Identity security teams
- Microsoft Defender XDR analysts
- Microsoft Entra administrators
- Detection engineers
- Security-awareness and blue-team trainers

## Responsible Use

Use this project only for defensive education, tabletop exercises, analyst training, and authorized security-awareness activities.

Do not modify it to collect credentials, capture authorization material, contact live identity endpoints, impersonate a real sign-in workflow without clear training disclosure, or facilitate unauthorized access.

## References

- [Mitiga: ConsentFix OAuth Phishing Explained](https://www.mitiga.io/blog/consentfix-oauth-phishing-explained-how-token-based-attacks-bypass-mfa-in-microsoft-entra-id)
- [Microsoft: Protect Against Consent Phishing](https://learn.microsoft.com/en-us/entra/identity/enterprise-apps/protect-against-consent-phishing)
- [Microsoft: Investigate Identity Risk Telemetry](https://learn.microsoft.com/en-us/entra/architecture/id-protection-guide-investigate)

## License

Choose a license that matches your intended distribution and your organization's requirements. For a broadly reusable defensive training project, the MIT License is a common option, but obtain any required organizational approval before publishing.

## Disclaimer

This project is provided for educational and defensive purposes only. The maintainers make no guarantee that the simulation reflects every implementation, tenant configuration, product behavior, or attack variation. Validate investigation and containment actions against current Microsoft documentation and your organization's approved procedures.
