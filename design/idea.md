# Idea

A non admin user of Kubernetes distribution cluster would not be able to create backups/restores through Velero/OADP, but would be able to create them through NAC operator.

NAC operator would perform the following actions:
- check if Velero/OADP is installed and running properly
- if user is asking to backups/restores through NAC operator, check if that user has permission over those resources
- if user does have permission, ask Velero/OADP to create that backup/restore, and return result of the operation to NAC operator

## Study

At first, the idea was to NAC operator to have OADP as a Dependency Operator, but that has two possible problems
- if non admin user could install NAC operator, user could also install OADP, contradicting the non admin approach of this solution
- users would want to install only one instance of OADP (in a specific admin namespace), but Operator SDK only allows Dependency Operator to be installed in the same namespace as the installing operator. Reference: https://olm.operatorframework.io/docs/concepts/olm-architecture/dependency-resolution/#caveats

Another problem to look, is which versions of NAC operator are compatible with which OADP versions.

I think the advantage of using Operator SDK is that is easy to ship the code, but since its based in kubebuilder, it takes longer to features to get into it.

## Suggestion implementation

Create one more CRD, that would check both if OADP is installed and running, and if OADP version is compatible with NAC operator. This CRD could have as parameters the namespace where OADP is installed and the prefix to store files in bucket. Users would only be able to create NonAdminBackup/NonAdminRestore if there is a "reconciled" instance of this CR.

When user creates NonAdminBackup, NAC operator checks if user has permission over all resources that would be back upped. If yes, NAC operator calls OADP. After operation ends, NonAdminBackup would have the same status as OADP backup.

When user creates NonAdminRestore, NAC operator checks if user is the owner of related NonAdminBackup. If yes, NAC operator calls OADP. After operation ends, NonAdminRestore would have the same status as OADP restore. (THIS SEEMS VERY RESTRICTIVE)

Which would imply in 3 controllers:
- one to the new CRD that would run in loop, checking every 5 minutes, for example, if everything is ok with OADP
- one for NonAdminBackup, check with first controller if everything is good; check if user has permission; ask OADP to run backup; run in loop, checking every 5 seconds (for example) if backup operation finished, if yes, finish loop and update NonAdminBackup status
- one for NonAdminRestore, check with first controller if everything is good; check if user has permission; ask OADP to run restore; run in loop, checking every 5 seconds (for example) if restore operation finished, if yes, finish loop and update NonAdminRestore status

## Questions

- how to check user/user permissions?
- non admin user can create Schedules with this approach?
- non admin user could run velero command?
