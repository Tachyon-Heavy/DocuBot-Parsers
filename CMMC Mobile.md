THINGS YOU MUST DO (Device/Environment Access Required)
Intune Configuration

 Configure compliance policy (encryption, password, timeout)
 Configure device configuration profile (Samsung Knox E-FIPS enabled)
 Configure Conditional Access policy (require compliant device + MFA)
 Set up Wi-Fi profile (WPA2-Enterprise with authentication)
 Configure enrollment restrictions (only authorized users)
 Enable audit logging in Intune
 Enable Azure AD sign-in logs

Device Management

 Factory reset all devices
 Enroll all devices in Intune
 Verify all devices show "Compliant" status
 Test remote wipe on one device
 Verify Knox E-FIPS mode is enabled on each device
 Assign devices to specific users in inventory
 Apply all available security patches/updates

FIPS Validation (CRITICAL - No POA&M)

 Verify Samsung Knox E-FIPS FIPS 140-2 certificate exists for Android 16

If NO: Downgrade to Android 15 (if validated) OR wait for validation OR don't use phones for CUI yet


 Document FIPS certificate number in evidence package
 Verify WiFi infrastructure uses FIPS 140-2 validated crypto

Check AP manufacturer's validation certificate


 Document WiFi FIPS certificate in evidence package

STIG Compliance (Must Be Complete)

 Download DoD Android STIG from public.cyber.mil
 Run STIG compliance scanner on sample device (e.g., STIG Viewer + manual checks)
 Remediate ALL findings
 Document compliance with each STIG requirement
 Re-scan to verify 100% compliance

Testing & Validation

 Test: Non-compliant device blocked from accessing CUI
 Test: Remote wipe successfully removes company data
 Test: MFA enforced on device enrollment
 Test: Encryption cannot be disabled by user
 Test: Failed login attempts trigger lockout
 Test: Audit logs capture device connections
 Test: Lost device procedure end-to-end

Evidence Collection

 Export device inventory from Intune
 Export compliance reports from Intune
 Export audit logs (last 30 days minimum)
 Export Azure AD sign-in logs
 Screenshot all Intune policies
 Screenshot Conditional Access policies
 Screenshot device compliance dashboard
 Export STIG scan results
 Document all test results with screenshots

Policy Approval

 Get management approval on all policies
 Get legal review on policies (if required)
 Distribute policies to all users
 Collect signed user acknowledgment forms from ALL users

Training

 Deliver security awareness training to all mobile users
 Train IT staff on device enrollment procedures
 Train IT staff on incident response procedures
 Document training completion (attendance sheets, certificates)

Network Infrastructure

 Document network diagram showing mobile device connectivity
 Configure firewall rules for mobile device access
 Ensure VPN (if used) has FIPS-validated encryption
 Document all network access control points

Physical Security (if devices stored on-site)

 Secure storage for spare/unassigned devices
 Document physical access controls
 Maintain check-in/check-out log


THINGS SUBCONTRACTOR CAN DO (No Access Required)
Policy Documentation

 Write Mobile Device Security Policy
 Write Acceptable Use Policy
 Write Lost/Stolen Device Procedure
 Write Incident Response Procedures (mobile-specific)
 Create user acknowledgment forms
 Create visitor/guest device policy (if applicable)

Standard Operating Procedures

 Device enrollment SOP
 Device de-provisioning SOP
 Lost/stolen device response SOP
 Monthly compliance review SOP
 STIG update/patching SOP
 Incident response SOP

Training Materials

 User security awareness training slides
 User quick reference guide
 IT administrator training guide
 Video tutorials (optional)
 FAQ document

Assessment Documentation

 Create assessment spreadsheet for all 110 requirements
 Map each requirement to Intune controls
 Write assessment objective findings (you provide evidence locations)
 Create evidence matrix template
 Document assessment methodology

System Security Plan (SSP)

 Write mobile device section of SSP
 Create data flow diagrams
 Document security control descriptions
 Map controls to NIST SP 800-171 requirements
 Document roles and responsibilities

Research & Documentation

 Research Samsung Knox security features
 Compile Samsung Knox security documentation
 Research DoD Android STIG requirements
 Find FIPS validation certificates (provide URLs/references)
 Compile NIST guidance references
 Create bibliography/reference list

SPRS Submission Package

 Create executive summary
 Format self-assessment results for SPRS
 Create compliance summary document
 Organize evidence into submission package
 Write cover letter/transmittal memo

Templates & Forms

 Device inventory template
 Incident report form
 Configuration change request form
 Monthly compliance review template
 User acknowledgment form
 Risk assessment template