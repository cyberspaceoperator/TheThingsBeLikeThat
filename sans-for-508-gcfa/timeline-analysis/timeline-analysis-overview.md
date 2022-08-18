# Timeline Analysis Overview

* 3 Core areas of focus for a timeline
* Filesystem Metadata
* Windows Artifacts
* Registry Keys

![windows artifacts](<../../.gitbook/assets/image (2).png>)

Where do I begin to look?

* Use your scope and case knowledge to help form that answer
* A timeline has many places where critical activity has occurred
* Called a timeline pivot point
  * What occurred right before and right after that pivot point?
* How do you determine the pivot point?

![pivot point](<../../.gitbook/assets/image (1).png>)

2 types of timelines covered in FOR-508

1. Filesystem (fls or MFTECmd)
   * Filesystem metadata only
   * More filesystem types
     * Apple (HFS)
     * Solaris(UFS)
     * Linux(EXT)
     * Windows(FAT/NTFS)
   * Wider OS/Filesystem capability
2. SuperTimeline (log2timeline)
   * Obtain everything (kitchen sink)
     * Filesystem metadata
     * Artifact timestamps
     * Registry timestamps
   * Windows, Linux, and Mac

### Timeline Analysis Process

1. Determine Timeline Scope: What questions are you trying to answer
2. Narrow pivot points
   * Time-based
   * Artifact-based
3. Determine the best process for timeline creation
   * Filesystem-Based Timeline creation
   * Super Timeline Creation (log2timeline)
4. Filter timeline
5. Analyze timelime
   * Focus on the context of the evidence
   * Use Windows Forensics "Evidence Of..." poster
