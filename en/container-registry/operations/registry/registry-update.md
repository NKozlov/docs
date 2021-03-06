# Updating a registry

Find out how to change:

* [The name of a registry](#update-name)
* [The label of a registry](#update-label)

To access the [registry](../../concepts/registry.md), use its ID or name. For information on how to find the registry ID or name, see [Getting information about existing registries](registry-list.md).

## Changing the name of a registry {#update-name}

---

**[!TAB CLI]**

[!INCLUDE [cli-install](../../../_includes/cli-install.md)]

Change the registry name:

```
$ yc container registry update my-reg --new-name new-reg
id: crp3qleutgksvd1prhvb
folder_id: b1g88tflru0ek1omtsu0
name: new-reg
status: ACTIVE
created_at: "2019-01-15T14:39:48.154Z"
```

**[!TAB API]**

To change the registry name, use the [update](../../api-ref/Registry/update.md) method for the [Registry](../../api-ref/Registry/) resource.

---

## Changing the label of a registry {#update-label}

---

**[!TAB CLI]**

[!INCLUDE [cli-install](../../../_includes/cli-install.md)]

Change the registry label (don't confuse this with Docker image tags):

```
$ yc container registry update new-reg --labels new_label=test_label
id: crp3qleutgksvd1prhvb
folder_id: b1g88tflru0ek1omtsu0
name: new-reg
status: ACTIVE
created_at: "2019-01-15T14:39:48.154Z"
labels:
  new_label: test_label
```

**[!TAB API]**

To change the registry label, use the [update](../../api-ref/Registry/update.md) method for the [Registry](../../api-ref/Registry/) resource.

---
