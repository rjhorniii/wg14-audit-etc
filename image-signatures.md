Concerns about images:
1. Has it been modified?
    * Intentionally, e.g., image enhancement
    * Unintentionally
    * Maliciously
2. How was it generated?
    * When, where, by what, etc.
    * What was authorization, ownership, etc.
3. Why was it generated?
    * Patient imaging for diagnosis, therapy, etc.
    * Training image
    * Phantom or calibration image
4. If derivative, what is its provenance/history/sources

Digital signatures can help with 1. and 2.  DICOM has some attribute level support for phantoms and calibration, which is part of 3. Provenance becomes extremely complex depending upon how much detail is needed in the provenance.  This partially addresses the concerns with artificially generated or modified images.

## DICOM Creator RSA Digital signature profile

The Creator RSA Digital signature profile https://dicom.nema.org/medical/dicom/current/output/chtml/part15/sect_C.2.html, is old but it remains relevant.  It details all the attributes of a image storage SOP Instance that are created by the imaging system and that should not normally be modified.  (Derived images from image enhancement were assumed to be a different SOP Instance.)  It specifies that these attributes should be signed, while leaving the remainder unsigned.  This means that information like patient demographics are not signed.

This profile should be reviewed to see:
1. Are there new image attributes that should be added to the list?  (How should we structure a profile like this so that it does not become invalid and need to replaced when new modality types introduce new attributes to be signed.  Is this a current problem or merely speculative?)
2. Are there any digital signature requirements that should be revised?  For example, it lists md5 as a signature method.  A reader of antique SOP instances may find signatures that used md5.  A signer of new SOP instances should not use md5 as its signature method.

Should signer information be required (or optional) to identify when, where, what, etc. information?

## Policy transition

I expect a policy to be established to require Creator signatures for all images.  This will take a while to transition.  As devices are updated they will start signing their images.  The recipients might ignore the signatures at first.  As devices are updated policy could change on the archives, workstations, etc. to start checking signatures.  During the transition period there would need to be adjustments to inform the recipients what devices are expected to be signing their SOP instances.  There are various ways this could be done, probably some variation of a local file that indicates instances from device X should start being signed on date X.

I expect the policy might be local, regional, or national.  It might be by discipline or a gradual legal mandate.

## Key Management

DICOM has remained silent on key management and certificate management issues.  Should this change?  It is not the area of expertise for DICOM, and it has no features that are unique to medical imaging.  Silence on this issue leaves medical imaging organizations to find their own solutions and standards.

There was an introduction made by "Safe Identity" to a system for key management for medical information.  The "Safe Identity" organization was acquired by DirectTrust in 2021.  DirectTrust can be found at: https://directtrust.org/.  At the time we referred them to MITA, because their effort looked much more relevant to trade association activities than to DICOM as an international standard.

The FIDO organization has made substantial progress on systems for providing certificates to the IoT world.  Their approach is relevant to all the imaging systems (CT, Ultrasound, etc.) and to a lesser degree to the workstations.  Their work can be found at: https://fidoalliance.org/internet-of-things/. FIDO got started with the standards for MFA tokens, etc. and their integration into browsers and web servers.  The IoT effort is a second or third stage effort.

The key used to sign a SOP Instance identifies the signer to some degree.  DICOM has been silent on what identification is appropriate.  The signer may provide provenance information.  

So far I've seen a variety of certificate distribution methods that are appropriate to IoT type devices.  Systems like Let's Encrypt currently expect the client system to be a server or server-like.  They depend on the client updating HTTP accessible information or DNS records.  This makes sense for a system that is designed to provide certificates to web servers.

For IoT style devices I've seen:
1. The vendors of the device installs a vendor-wide signing certificate.  This provides only the vendor of the device as provenance information.  It does not indicate device type, device identification, or owner identification information.
2. Some vendors have a service activity to provide site and machine specific certificates.  This does provide all that identification information, but the owner cannot revoke or modify that certification.
3. Some vendors allow machine owners to reference the owner's internal certificate authority for authorizetion or identification.
4. FIDO is attempting to provide some degree of this information for devices owned and operated by naive consumers.  Their intent is to identify each device uniquely, and accomodate consumer level disabling, sale, and transfer of their devices.

Should DICOM get into this?  Does this better belong with regulators (and thus organizations like MITA, COCIR, and JIRA that represent medical imaging to regulators)?  Should IHE get involved?  Is this not a medical imaging specific problem, and should it be left to the wider industry to solve.

### Relevance to artificially generated or modified images

The system that creates the new SOP instances will either have to sign the SOP instances or be flagged as an unverified source.  If it is signed and we use signatures that are uniquely assigned to a particular device, that identifies the source.  If that source is capable of both imaging and generation of artificial images, the problem of identifying artificial images is not solved.  But, in most cases, identifying the device that created the image will be sufficient to detect artificial or modified images.

This also assumes that there is adequate protection for the private signing certificates.  If those are disclosed, this breaks down.