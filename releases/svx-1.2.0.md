### Securing user invitation endpoints

Following checks are performed when inviting users to join global, tenant or organisation admins.

* `atom:global:assign_global_sr_to_user` - this one is for adding global admins and removing global admins
* `global:assign_tenant_sr_to_tenant_admin` `tenant:assign_tenant_sr_to_tenant_admin`  - these two are for adding tenant admins and removing tenant admins. global:assign_tenant_sr_to_tenant_admin enables a global admin to do it, tenant:assign_tenant_sr_to_tenant_admin enables a tenant admin to do it
atom:global:assign_org_sr_to_user atom:tenant:assign_org_sr_to_user  atom:org:assign_org_sr_to_user - these three are for adding org admins and removing org admins. atom:global:assign_org_sr_to_user enables a global admin to do it, atom:tenant:assign_org_sr_to_user enables a tenant admin to do it, atom:org:assign_org_sr_to_user enables an org admin to do it