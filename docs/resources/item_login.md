---
# generated by https://github.com/hashicorp/terraform-plugin-docs
page_title: "bitwarden_item_login Resource - terraform-provider-bitwarden"
subcategory: ""
description: |-
  Use this resource to set (amongst other things) the username and password of a Bitwarden Login item.
---

# bitwarden_item_login (Resource)

Use this resource to set (amongst other things) the username and password of a Bitwarden Login item.

## Example Usage

```terraform
resource "bitwarden_item_login" "administrative-user" {
  name            = "Service Administrator"
  username        = "admin"
  password        = "<sensitive>"
  totp            = "<sensitive>"
  notes           = "some notes about this user"
  folder_id       = "3b985a2f-0eed-461e-a5ac-adf5015b00c4"
  organization_id = "54421e78-95cb-40c4-a257-17231a7b6207"
  favorite        = true
  collection_ids  = ["c74d6067-50b0-4427-bec8-483f3270fde3"]

  field {
    name = "this-is-a-text-field"
    text = "text-value"
  }

  field {
    name    = "this-is-a-boolean-field"
    boolean = true
  }

  field {
    name   = "this-is-a-hidden-field"
    hidden = "text-value"
  }
}
```

<!-- schema generated by tfplugindocs -->
## Schema

### Required

- `name` (String) Name.

### Optional

- `collection_ids` (List of String) Identifier of the collections the item belongs to.
- `favorite` (Boolean) Mark as a Favorite to have item appear at the top of your Vault in the UI.
- `field` (Block List) Extra fields. (see [below for nested schema](#nestedblock--field))
- `folder_id` (String) Identifier of the folder.
- `notes` (String) Notes.
- `organization_id` (String) Identifier of the organization.
- `password` (String, Sensitive) Login password.
- `reprompt` (Boolean) Require master password “re-prompt” when displaying secret in the UI.
- `totp` (String) Verification code.
- `username` (String) Login username.

### Read-Only

- `id` (String) Identifier.
- `revision_date` (String) Last time the item was updated.

<a id="nestedblock--field"></a>
### Nested Schema for `field`

Required:

- `name` (String)

Optional:

- `boolean` (Boolean)
- `hidden` (String)
- `linked` (String)
- `text` (String)


