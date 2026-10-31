3. **Always use `DefaultAzureCredential`**; never hardcode credentials
6. **Suspend when not in use**; Fabric capacities bill for compute even when idle
8. **Use appropriate SKU**; Start small (F2/F4) for dev/test, scale up for production
1. **Capacity names must be globally unique**; Fabric capacity names must be unique across all Azure subscriptions
2. **Suspend doesn't delete**; Suspended capacities still exist but don't bill for compute
3. **SKU changes may require downtime**; Scaling operations can take several minutes
4. **Admin UPNs must be valid**; Capacity administrators must be valid Azure AD users
5. **Location constraints**; Not all SKUs are available in all regions; use `GetSkusFabricCapacitiesAsync` to check
6. **Long provisioning times**; Capacity creation can take 5-15 minutes