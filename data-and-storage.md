# Storage

## Directory Structure

To promote long term consistency, the S3DF directory structure provides immutable paths, independent from the underlying file system organization and technology:

* `/sdf`: Root mount point.

* `/sdf/home/<u>/<username>`: Home directories. Space quotas imposed for all users.

* `/sdf/sw/<package>/<version>`: For general purpose software not installed on each node, e.g., EPICS, Matlab, matplotlib, GEANT4, etc. Not meant for software that is used by only one group.

* `/sdf/group/<organization>/<groupname>` or `/sdf/group/<groupname>`: For group/project specific software (e.g., lcls/psdm, ad/hla, etc.) Space quotas imposed for all groups.

* `/sdf/data/<facility>/…`: For science data (as opposed to code, documents, etc), including raw, calibrated data, and results. Some examples:
  - LCLS experimental: `/sdf/data/lcls/<instrument>/<experiment>`
  - LCLS accelerator: `/sdf/data/lcls/accel/<bld|bsa|ca|...>`
  - FACET experimental: `/sdf/data/facet/<instrument>/<experiment>`
  - FACET accelerator: `/sdf/data/facet/accel/`
  - CryoEM: `/sdf/data/cryoem/<YYYYMM>/<experiment>`

* `/sdf/scratch/<facility>/…`: 3 months retention on a best effort basis (actual retention can be shorter or longer depending on actual usage). Space quotas imposed for all facilities.

* `/sdf/scratch/users/...`: 1 month retention on a best effort basis (actual retention can be shorter or longer depending on actual usage). Space quotas imposed for all users.

?> Access to AFS, GPFS, and SDF Lustre from S3DF is described in this
[reference section on legacy file systems](reference.md#legacyfs).

## Policies

- Home directory permissions will be delegated to each user. By default, home folders will be readable by everyone, though you can change that by changing UNIX permissions on one or more of your folders. Everyone will be able to list `/sdf/home/<u>`.

- General purpose software will go under sw. Group specific software will go under `/sdf/group` and will be maintained by each group.

- Some groups may decide to logically hold all their information under `/sdf/group/<groupname>`. Such a structure may be implemented by each group via symlinks. The actual mount points and relative backup and archive policies will be based on the structure shown above. 

?> __TODO__ Desktop/endpoint access to S3DF file systems will likely be via authenticated NFS v4.  This is currently a topic of investigation as we wait for an updated WekaFS release.


## Backup and Archiving

- Everything under `/sdf/{home, sw, group}` will be backed up. We currently use snapshots taken at regular intervals (e.g., a few times a day) that are accessible with no intervention from the system administrators. A subset of the snapshots will be copied to tape at a lower rate (e.g., once a day). Snapshots for\
`/sdf/{home, sw, group}/<folder>`\
can be found at\
`/sdf/{home, sw, group}/.snapshots/<gmttime>/<folder>`

- Files/objects under `/sdf/data` will be archived according to a data retention policy defined by the facility. Facilities will be responsible for covering the media costs and overhead required by their policy. Derived data (results) may be backed up, or archived, or somewhere in between - TBD.

- The scratch spaces under `/sdf/scratch` and directories named "nobackup" (located *anywhere* in an /sdf path) will not be backed up or archived. Please use as many "nobackup" subdirectory locations as required for any files that do not need backup.  That can save significant tape and processing resources.

- A subset of users in each group will be able to access the command line interface to HPSS for the purpose of archiving/retrieving data to/from tape. Unlike backups, which will be automatically performed by the storage team within SCS, archiving will be the responsibility of each group (contact SCS for assistance).


?> The current and target backup and archiving policies are summarized in this [reference section on data backup](reference.md#backup).

