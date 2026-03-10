## 🧠 What Are WordPress User Roles and Capabilities?

### ✅ **User Roles**:

WordPress uses a **role-based access control system** to assign different sets of permissions (called _capabilities_) to users.

Each role has a specific set of capabilities.

### ✅ **Capabilities**:

These are individual permissions such as:

- `edit_posts`
    
- `publish_pages`
    
- `delete_users`
    
- `install_plugins`
    

---

## 🔑 Default WordPress User Roles

|Role|Description|
|---|---|
|**Administrator**|Full access. Can manage the entire site, themes, plugins, users, etc.|
|**Editor**|Can manage and publish posts and pages (own and others).|
|**Author**|Can write, edit, and publish only their own posts.|
|**Contributor**|Can write and edit their own posts but **not publish** them.|
|**Subscriber**|Can only read content and manage their profile (used for memberships).|

---

## 🔧 How to Manage or Customize Roles

### 🧩 1. **Using Plugins (Easiest)**

Install **User Role Editor** plugin:

sql

CopyEdit

`Plugins → Add New → Search: "User Role Editor" → Install → Activate`

With it, you can:

- Add new roles
    
- Modify capabilities
    
- Assign custom roles to users
    

---

### ⚙️ 2. **Manual (Code-Based) Role & Capability Management**

You can add roles and capabilities via `functions.php` or a custom plugin.

#### 🔸 Add a Custom Role:

php

CopyEdit

`add_role(     'project_manager',     'Project Manager',     [         'read' => true,         'edit_posts' => true,         'delete_posts' => false,         'manage_categories' => true,     ] );`

#### 🔸 Add/Remove Capability to a Role:

php

CopyEdit

`// Add a capability to Editor $role = get_role('editor'); $role->add_cap('edit_theme_options');  // Remove a capability $role->remove_cap('delete_others_posts');`

#### 🔸 Assign a Role to a User:

php

CopyEdit

`$user = new WP_User($user_id); $user->set_role('editor');`

---

## 📌 Role Hierarchy (Security Best Practices)

- **Administrator**: Never assign to users unless they must manage the whole site.
    
- **Editor**: Suitable for content managers.
    
- **Author**/**Contributor**: Best for writers or bloggers.
    
- **Subscriber**: Minimal access, best for members-only sites.
    

---

## 🛡 Best Practices

- Keep **admin access limited**
    
- Use **custom roles** for fine control
    
- Always **back up your site** before altering roles/capabilities programmatically
    

---

Would you like me to generate code for a specific custom role based on your needs (e.g., limited admin or advanced contributor)?
