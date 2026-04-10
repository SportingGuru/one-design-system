# ONE JavaScript Patterns — Behavioral Reference

## Overview

Companion to `one-design-system.css` and `ONE-DESIGN-SYSTEM.md`. This document captures the **behavioral layer** — icons, interaction flows, data shapes, state management, and Skuid integration patterns — extracted from two production components:

- **Patient Messaging** (`pm-` prefix) — template-driven SMS sending with multi-language support
- **Calendar Preference Selector** (`cps-` prefix) — drag-and-drop clinician list builder

Use this alongside the CSS design system when building new ONE components so they look **and behave** consistently.

---

## 1. Icon Catalog

All icons are inline SVGs using Lucide-style stroked paths. No icon font or external library — each icon is a self-contained `<svg>` string stored in an `ICONS` object at the top of each component.

### Sizing Conventions

| Context | Width/Height | Examples |
|---|---|---|
| Tags, badges, inline labels | 12–14px | sparkles, info, eye, chevronRight (small) |
| Buttons, form controls | 14–16px | x (close), check, edit2, trash2, pin, save |
| Search bars, dropdown triggers | 16–18px | search, chevronDown, building, loader |
| Headers, feature icons | 18–20px | messageSquare, settings, user (large), warning |
| Template category icons | 18px | All TEMPLATE_ICONS (MapPin, Calendar, Video, etc.) |

### Shared Icon Set (appear in both components)

```javascript
// These icons are used across multiple components and should be
// considered the "core" ONE icon vocabulary.
var OTN_SHARED_ICONS = {
    user:        /* 14–20px */ 'person silhouette — avatars, personal visibility',
    search:      /* 16–18px */ 'magnifying glass — search inputs',
    chevronDown: /* 14–18px */ 'dropdown trigger, collapsible toggle',
    chevronLeft: /* 18px */    'back navigation',
    chevronRight:/* 12–20px */ 'forward nav, submenu indicator',
    x:           /* 16–18px */ 'close button, remove action',
    check:       /* 16px */    'selected state, confirmation',
    building:    /* 14–18px */ 'clinic/location context',
    loader:      /* 16–18px */ 'spinner — add class "pm-spinner" or "cps-spinner" for rotation',
    warning:     /* 14–20px */ 'triangle alert — warnings, unsaved changes (aliased as alertTriangle in PM)'
};
```

### Patient Messaging Specific Icons

```javascript
// PM-only icons — messaging, template management, patient info
var PM_ICONS = {
    messageSquare: /* 18-20px */ 'primary component icon, template default',
    send:          /* 16px */    'send button, confirm send',
    checkCircle:   /* 16px */    'success state, fields resolved, SMS ready',
    phone:         /* 16px */    'phone number section, recipient display',
    globe:         /* 14px */    'language selector label',
    pin:           /* 14px */    'pinned template indicator (amber #f59e0b)',
    pinOff:        /* 14px */    'unpin action',
    sparkles:      /* 12px */    'suggested template badge (blue #3b82f6)',
    folderOpen:    /* 12px */    'browse all tab',
    alertTriangle: /* 14px */    'opt-in warning, unresolved tokens, blocked send',
    info:          /* 12px */    'hint text, segment count note',
    users:         /* 14px */    'shared/global visibility',
    stethoscope:   /* 14px */    'clinician role badge',
    briefcase:     /* 14px */    'clinic director role badge',
    settings:      /* 18px */    'manage templates button',
    plus:          /* 16px */    'new template, insert field',
    edit2:         /* 14px */    'edit action on cards',
    trash2:        /* 14px */    'delete action',
    save:          /* 14px */    'editor save button',
    eye:           /* 12px */    'preview toggle',
    inbox:         /* 14px */    'all templates folder, empty state',
    shieldCheck:   /* 14px */    'system template, opt-in status, admin badge',
    shieldOff:     /* 14px */    'opt-out / not authorized',
    userCircle:    /* 16px */    'patient info button',
    calendar:      /* 14px */    'appointment fields',
    refresh:       /* 14px */    'admin refresh button'
};
```

### Calendar Preference Selector Specific Icons

```javascript
// CPS-only icons — list management, reordering
var CPS_ICONS = {
    chevronsRight: /* 20px */ 'add all — double arrow transfer',
    chevronsLeft:  /* 20px */ 'remove all — double arrow transfer',
    grip:          /* 16px */ 'drag handle — 6-dot pattern for reorder',
    arrowUp:       /* 14px */ 'move up in list (compact reorder button)',
    arrowDown:     /* 14px */ 'move down in list (compact reorder button)',
    undo:          /* 16px */ 'cancel/discard changes'
};
```

### Template Category Icons (PM only)

These are larger (18px) icons used on template cards, function headers, and the icon picker in the editor:

```javascript
var TEMPLATE_ICONS = {
    MapPin:         'location-based templates',
    Calendar:       'scheduling, dr. appointments function',
    CalendarCheck:  'confirmed appointments',
    Video:          'teletherapy/virtual visit',
    Clock:          'time-related, reminders',
    ClipboardCheck: 'evals function',
    UserPlus:       'intakes function, new patient',
    FileText:       'document/form templates',
    FileCheck:      'eligibility function, verified docs',
    CheckCircle:    'success/completion templates',
    Scale:          'probation function, legal',
    Star:           'general purpose, featured',
    MessageSquare:  'default template icon, general messaging'
};

// Icon picker groups (for template editor)
var ICON_CATEGORIES = {
    'Location':  ['MapPin'],
    'Time':      ['Calendar', 'CalendarCheck', 'Clock'],
    'Virtual':   ['Video'],
    'Medical':   ['ClipboardCheck'],
    'People':    ['UserPlus'],
    'Forms':     ['FileText', 'FileCheck'],
    'General':   ['CheckCircle', 'Star', 'MessageSquare'],
    'Probation': ['Scale']
};
```

### Spinner Animation

Both components use the same spinner pattern — a partial-circle SVG with a CSS rotation:

```css
/* Applied via class on the <svg> element itself */
.pm-spinner, .cps-spinner {
    animation: spin 1s linear infinite;
}
@keyframes spin {
    from { transform: rotate(0deg); }
    to { transform: rotate(360deg); }
}
```

---

## 2. Discipline Color System

### Color Map

Both components use a discipline→color lookup, but with slightly different keys and values. Here's the **consolidated** map:

```javascript
// Canonical discipline color definitions
// Key = Salesforce Discipline__c picklist value
var DISCIPLINE_COLORS = {
    'Physical':     { bg: '#dcfce7', text: '#166534', border: '#86efac', abbrev: 'PT' },
    'Occupational': { bg: '#f3e8ff', text: '#6b21a8', border: '#d8b4fe', abbrev: 'OT' },
    'Speech':       { bg: '#e0f2fe', text: '#0369a1', border: '#7dd3fc', abbrev: 'ST' },
    'Behavioral':   { bg: '#fce7f3', text: '#9d174d', border: '#f9a8d4', abbrev: 'ABA' },
    'Counseling':   { bg: '#fef3c7', text: '#92400e', border: '#fcd34d', abbrev: 'Counseling' }
};

var DEFAULT_DISCIPLINE_COLOR = { bg: '#f3f4f6', text: '#374151', border: '#d1d5db', abbrev: '?' };
```

> **Note:** The CSS design system documents a slightly different mapping (using the common abbreviation keys ABA/SLP/OT/PT/MH). The JS source uses the Salesforce picklist values as keys. When building new components, use the Salesforce picklist value as the lookup key and fall back to `DEFAULT_DISCIPLINE_COLOR`.

### Runtime Application Pattern

Colors are applied inline via `style` attributes, not CSS classes. This is the standard pattern:

```javascript
// Badge on a clinician card
var colors = DISCIPLINE_COLORS[clinician.discipline] || DEFAULT_DISCIPLINE_COLOR;

var badgeHtml = '<span class="one-badge" style="' +
    'background: ' + colors.bg + '; ' +
    'color: ' + colors.text + '; ' +
    'border: 1px solid ' + colors.border + ';">' +
    colors.abbrev +
'</span>';

// Filter chip (active state)
var chipStyle = isActive
    ? 'background: ' + colors.bg + '; color: ' + colors.text + '; border-color: ' + colors.border + ';'
    : '';

// Discipline group header (CPS grouped view)
var headerHtml = '<div class="cps-discipline-header" style="' +
    'background: ' + colors.bg + '; border-color: ' + colors.border + ';">' +
    '<span style="color: ' + colors.text + ';">' + colors.abbrev + '</span>' +
'</div>';
```

### Discipline Sort Order

When grouping or sorting by discipline, use this canonical order:

```javascript
var DISCIPLINE_ORDER = ['Physical', 'Occupational', 'Speech', 'Behavioral', 'Counseling'];
// Unknown disciplines sort to the end
```

---

## 3. Data Shapes

### Clinician Record (CPS)

```javascript
// Available clinician (from territory association query)
{
    id: 'UserId',           // Salesforce User.Id
    name: 'Jane Smith',     // User.Name
    discipline: 'Speech',   // User.Discipline__c (picklist value)
    photoUrl: '/photo/...'  // User.SmallPhotoUrl (nullable)
}

// Selected/working clinician (in preference list)
{
    prefId: 'a0X...',        // Calendar_Preference__c.Id (null if newly added)
    clinicianId: 'UserId',   // Salesforce User.Id
    name: 'Jane Smith',
    discipline: 'Speech',
    photoUrl: '/photo/...',
    displayOrder: 3          // 1-based position in list
}
```

### Clinic Record (CPS)

```javascript
{
    id: 'AccountId',
    name: 'Sunrise Therapy - Austin',
    territoryId: 'TerritoryId'  // Used to filter clinicians
}
```

### Message Template (PM)

```javascript
{
    id: 'SalesforceRecordId',
    sfRow: { /* raw Skuid model row */ },
    name: 'Record Name',
    title: { en: 'English Title', es: 'Spanish Title', other: '' },
    message: { en: 'Hello {{Patient.FirstName}}...', es: 'Hola {{Patient.FirstName}}...', other: '' },
    icon: 'Calendar',              // Key into TEMPLATE_ICONS
    roles: ['front_desk', 'clinician'],  // Mapped from picklist
    functions: ['General', 'Intakes'],   // Raw picklist values
    isGlobal: true,                // Shared visibility
    isSystem: false,               // System-managed (admin only)
    sortOrder: 5,                  // Sort_Order__c
    isPinned: true,                // From User_Pinned_Template__c junction
    missingContext: ['appointment'],// Context groups needed but not available
    score: 30                      // Relevance score for suggestion ranking
}
```

### Context Object (PM)

Built at component init from Skuid models. Provides token resolution data:

```javascript
{
    role: 'front_desk',  // Resolved from UserRole.Name → ROLE_MAP
    patient: {
        firstName: '', lastName: '', name: '',
        phone: '', mobilePhone: '', homePhone: '',
        language: '',      // Text_Language__c
        textOptIn: ''      // Text_Opt_In_Picklists__c ('Yes'|'No'|'')
    },
    clinic: {
        name: '', phone: '', fax: '', email: '', address: ''
    },
    provider: {
        name: '', credentials: '', employeeType: '', virtualLink: ''
    },
    appointment: {
        date: '', time: '', dateTime: '', endTime: '',
        type: '', virtualLink: ''
    },
    authorization: { caseType: '' },
    sender: { name: '', credentials: '' }
}
```

### Phone Option (PM)

```javascript
{
    key: 'mobile',                  // 'mobile'|'primary'|'home'|'other'
    number: '5551234567',           // Raw from Salesforce
    label: 'Mom Cell',             // Description field or default
    formatted: '(555) 123-4567'    // Display format
}
```

### Role / Function Constants (PM)

```javascript
// Roles — map to Salesforce picklist values
var ROLES = [
    { id: 'front_desk',     label: { en: 'Front Desk' },      color: '#2563eb', picklistValue: 'Front Desk' },
    { id: 'clinic_director', label: { en: 'Clinic Director' }, color: '#7c3aed', picklistValue: 'Clinic Director' },
    { id: 'clinician',      label: { en: 'Clinician' },        color: '#16a34a', picklistValue: 'Clinician' }
];

// Functions — template categories
var FUNCTIONS = [
    { id: 'general',         label: { en: 'General' },          color: '#374151' },
    { id: 'eligibility',     label: { en: 'Eligibility' },      color: '#d97706' },
    { id: 'dr_appointments', label: { en: 'Dr. Appointments' }, color: '#2563eb' },
    { id: 'evals',           label: { en: 'Evals' },            color: '#4f46e5' },
    { id: 'intakes',         label: { en: 'Intakes' },          color: '#16a34a' },
    { id: 'probation',       label: { en: 'Probation' },        color: '#dc2626' }
];

// Role icon mapping
var ROLE_ICONS = {
    front_desk: 'building',
    clinic_director: 'briefcase',
    clinician: 'stethoscope',
    all: 'settings'
};

// Function icon mapping (keys into TEMPLATE_ICONS)
var FUNCTION_ICONS = {
    general: 'MessageSquare',
    eligibility: 'FileCheck',
    dr_appointments: 'Calendar',
    evals: 'ClipboardCheck',
    intakes: 'UserPlus',
    probation: 'Scale'
};
```

---

## 4. State Management Patterns

### Dirty/Unsaved Change Detection

Both components use JSON serialization comparison:

```javascript
// On load — snapshot the original state
this.originalSelected = JSON.parse(JSON.stringify(this.workingSelected));

// On every mutation — compare current vs original
checkForChanges: function() {
    this.hasUnsavedChanges =
        JSON.stringify(this.workingSelected) !== JSON.stringify(this.originalSelected);
}

// PM editor variant — excludes sfRow (non-serializable/circular)
this.editorFormSnapshot = JSON.stringify(this.editorForm, function(key, val) {
    return key === 'sfRow' ? undefined : val;
});
isEditorDirty: function() {
    return JSON.stringify(this.editorForm, replacer) !== this.editorFormSnapshot;
}
```

### View Routing

PM uses a string-based view state with a single `render()` router:

```javascript
// Possible views: 'send' | 'manage' | 'editor' | 'admin'
this.view = 'send';

render: function() {
    this.parent.innerHTML = '';
    var container = document.createElement('div');

    if (this.view === 'manage') this.renderManageView(container);
    else if (this.view === 'editor') this.renderEditorView(container);
    else if (this.view === 'admin') this.renderAdminView(container);
    else this.renderSendView(container);

    this.parent.appendChild(container);

    // Overlays render on top regardless of view
    if (this.showConfirmation) this.parent.appendChild(this.renderConfirmation());
    if (this.showSuccess) this.parent.appendChild(this.renderSuccessToast());
    // ... etc
}
```

### Full Re-render Pattern

Both components do full `innerHTML` rebuilds on each state change (no virtual DOM). This means:

- Event listeners must be re-attached after every `render()` call
- Focus/cursor position must be manually preserved (see Search below)
- DOM elements should not be cached across renders

---

## 5. Interaction Patterns

### 5.1 Confirmation Flows

**CPS — Unsaved Changes on Clinic Switch (3-button modal)**

```
User clicks different clinic
  → hasUnsavedChanges? 
    YES → show modal:
      [Cancel]          → close modal, stay on current clinic
      [Discard Changes] → reset workingSelected to original, switch clinic
      [Save & Switch]   → save() → then switch clinic
    NO  → switch immediately
```

**PM — Send Confirmation (2-step with token gate)**

```
User clicks "Review & Send"
  → showConfirmation = true → render overlay
  → Has unresolved {{tokens}}?
    YES → show "Message Not Ready" header (amber)
        → "Cannot Send" button (disabled, red)
        → "Go Back & Fix" button
    NO  → show "Confirm Message" header (blue)
        → "Send Now" button (active)
        → Click "Send Now":
            isSending = true → spinner in button, buttons disabled
            → AJAX call
            → Success: showSuccess = true → toast → 1800ms → auto-close popup
            → Error: blockUI error message → 4000ms
```

**PM — Delete Confirmation (2-button modal)**

```
Click delete → deleteTarget = template → render overlay
  [Cancel] → deleteTarget = null
  [Delete] → deleteTemplate(id) → save → refresh → re-render
```

**PM — Unsaved Editor Warning (2-button modal)**

```
Click back in editor → isEditorDirty()?
  YES → showEditorUnsavedWarning = true → render overlay
    [Keep Editing] → close overlay
    [Discard Changes] → navigate to manage/admin view
  NO  → navigate immediately
```

### 5.2 Toast / Feedback Timing

| Feedback Type | Duration | Mechanism | Auto-action |
|---|---|---|---|
| Success toast (PM send) | 1800ms | Floating `.pm-success-toast` | Closes entire popup after timeout |
| Save success (CPS) | 2000ms | `skuid.$.blockUI` | None — just dismisses |
| Cancel/discard (CPS) | 1500ms | `skuid.$.blockUI` | None |
| Error message (PM send) | 4000ms | `skuid.$.blockUI` with red styling | None |
| Error message (CPS/PM save) | 3000ms | `skuid.$.blockUI` | None |
| Opt-in saved flash (PM) | 600ms | Inline status text swap | Re-renders component |

**blockUI pattern:**
```javascript
skuid.$.blockUI({
    message: 'Preferences saved successfully!',
    timeout: 2000
});

// Error variant with custom styling
skuid.$.blockUI({
    message: '<div style="padding:20px;font-size:14px;color:#991b1b;">' +
             '<strong>Send Failed</strong><br/>' + errorMsg + '</div>',
    timeout: 4000,
    css: { border: '2px solid #fecaca', borderRadius: '12px', backgroundColor: '#fef2f2' }
});
```

### 5.3 Loading States

**Pattern: boolean flag → conditional render → flag reset**

```javascript
// Button loading state
this.isSaving = true;
this.render();
// Button now shows: ICONS.loader + ' Saving...' and is disabled

// Content area loading
this.isLoadingClinicians = true;
this.render();
// Panel now shows: ICONS.loader + ' Loading clinicians...' (empty state style)

// Full-page loading (admin)
this.adminLoading = true;
this.render();
// Shows: spinner div + 'Loading all messages...'
```

**Loading state inventory:**

| Flag | Component | Affects |
|---|---|---|
| `isSaving` | CPS | Save button → spinner + "Saving..." |
| `isLoadingClinicians` | CPS | Available panel → spinner + "Loading clinicians..." |
| `isSending` | PM | Confirm dialog send button → spinner + "Sending..." |
| `isSavingTemplate` | PM | Editor save button → spinner + "Saving..." |
| `adminLoading` | PM | Entire admin view → centered spinner |
| `isSavingOptIn` | PM | Opt-in status → spinner + "Saving...", buttons disabled |

### 5.4 Drag and Drop with Auto-Scroll (CPS)

Full native HTML5 drag-and-drop with edge-detection auto-scroll:

```
dragstart → store draggedIndex, add .cps-card-dragging class
  → store reference to scroll container

dragover → preventDefault, set dropEffect
  → Auto-scroll detection:
    - Mouse within 60px of container top edge → scroll up at 8px/frame (~60fps)
    - Mouse within 60px of container bottom edge → scroll down
    - Add .cps-scroll-zone-top or .cps-scroll-zone-bottom class
  → Add .cps-card-drop-target to hovered card

drop → splice item from old position, insert at new position
  → Recalculate displayOrder (1-based) for all items
  → checkForChanges() → render()

dragend → clear all intervals, remove all drag classes, null out state
```

Key implementation detail — the auto-scroll runs on a `setInterval(fn, 16)` (~60fps) and must be cleaned up on both `drop` and `dragend`.

### 5.5 Search with Cursor Preservation

Both components re-render the entire DOM on search input, so the cursor position must be restored:

```javascript
searchInput.addEventListener('input', function(e) {
    self.availableSearch = e.target.value;
    self.render();  // Full re-render destroys the input

    // Restore focus and cursor after DOM rebuild
    setTimeout(function() {
        var input = self.parent.querySelector('.cps-search-input');
        if (input) {
            input.focus();
            input.setSelectionRange(input.value.length, input.value.length);
        }
    }, 0);
});
```

### 5.6 Dropdown Menus

All dropdowns use the same pattern: toggle `display: none/block`, close on document click:

```javascript
// Open/close toggle
btn.addEventListener('click', function(e) {
    e.stopPropagation();  // Prevent document click from immediately closing
    menu.style.display = menu.style.display === 'none' ? 'block' : 'none';
});

// Global close-all on document click
document.addEventListener('click', function() {
    self.parent.querySelectorAll('.cps-clinic-menu, .cps-sort-menu')
        .forEach(function(m) { m.style.display = 'none'; });
});
```

---

## 6. Template Token System (PM)

### Token Format

Tokens use double-brace syntax: `{{Group.Field}}`

### Token Groups and Fields

```javascript
var FIELD_GROUPS = [
    { key: 'patient',       fields: ['FirstName', 'LastName', 'Name', 'Phone', 'MobilePhone', 'HomePhone'] },
    { key: 'clinic',        fields: ['Name', 'Phone', 'Fax', 'Email', 'Address'] },
    { key: 'provider',      fields: ['Name', 'Credentials', 'EmployeeType', 'VirtualLink'] },
    { key: 'appointment',   fields: ['Date', 'Time', 'DateTime', 'EndTime', 'Type', 'VirtualLink'] },
    { key: 'authorization', fields: ['CaseType'] },
    { key: 'sender',        fields: ['Name', 'Credentials'] }
];
```

### Token Resolution Pipeline

```
1. Template selected → get message text for current language
2. populateTokens(text, context, language)
   → For each DYNAMIC_FIELD, replace {{Token}} with context value
   → Special case: Authorization.CaseType gets Spanish translation in 'es' mode
3. Result stored in customMessage (editable by user)
4. On send: check for remaining {{...}} patterns
```

### Unresolved Token Detection

```javascript
// Check if any tokens remain after population
function hasUnresolvedTokens(message) {
    return /\{\{[^}]+\}\}/.test(message);
}

// Extract list of unresolved tokens
function getUnresolvedTokens(message) {
    return message.match(/\{\{[^}]+\}\}/g) || [];
}

// Visual highlighting — wraps each token in red-styled span
function highlightUnresolvedTokens(message) {
    return escapeHtml(message).replace(/\{\{[^}]+\}\}/g, function(match) {
        return '<span class="pm-token-unresolved">' + match + '</span>';
    });
}
```

### Context Availability Detection

Templates declare which context groups they need. The component checks what's available:

```javascript
// What does this template's message require?
function getRequiredContextGroups(messageText) {
    var groups = [];
    if (/\{\{Appointment\./.test(messageText)) groups.push('appointment');
    if (/\{\{Provider\./.test(messageText))    groups.push('provider');
    if (/\{\{Clinic\./.test(messageText))      groups.push('clinic');
    if (/\{\{Authorization\./.test(messageText)) groups.push('authorization');
    return groups;
    // Patient and Sender are always available — not checked
}

// What context do we actually have?
function getAvailableContextGroups(ctx) {
    var available = [];
    if (ctx.appointment.date || ctx.appointment.time) available.push('appointment');
    if (ctx.provider.name) available.push('provider');
    // ...etc
    return available;
}

// Templates with missing context get:
// - .pm-tpl-unavailable class (grayed out)
// - "Needs Appointment" label on card
// - Still selectable, but tokens won't resolve
```

### Template Scoring / Suggestion Engine

Templates are ranked by relevance to the current user's role and launch function:

```javascript
function scoreTemplate(tpl, role, launchFunction, isPinned) {
    var score = 0;
    if (isPinned) score += 100;                              // Pinned always on top
    if (roles match or empty or admin) score += 10;          // Role relevance
    if (functions match or has General or empty) score += 10; // Function relevance
    return score;
}
// score >= 20 → "Suggested" tab
// score < 20  → "Browse All" only
```

---

## 7. Skuid Integration Patterns

### Model Access

```javascript
// Get model reference
this.prefsModel = skuid.model.getModel('calendar_prefs_cps');

// Read rows
var rows = this.prefsModel.getRows();
var firstRow = this.prefsModel.getFirstRow();

// Filter rows in JS (no server round-trip)
var filtered = rows.filter(function(row) {
    return row.Clinic__c === clinicId && row.Is_Active__c === true;
});

// Read condition value (for launch parameters)
var condition = this.clinicsModel.getConditionByName('default_clinic_param');
var value = condition ? condition.value : null;
```

### Model Mutations

```javascript
// Update existing row
this.prefsModel.updateRow(existingRow, {
    Is_Active__c: true,
    Display_Order__c: newOrder
});

// Create new row
var newRow = this.prefsModel.createRow();
this.prefsModel.updateRow(newRow, { /* field values */ });

// Delete row
this.prefsModel.deleteRow(row);

// Save all pending changes to server
this.prefsModel.save({
    callback: function(result) {
        var isSuccess = result.totalsuccess || result.totalSuccess || result.success;
        if (isSuccess) { /* refresh and re-render */ }
        else { /* handle errors */ }
    },
    error: function(error) { /* handle errors */ }
});
```

### Model Requery (Server Refresh)

```javascript
// Requery with updated conditions
var condition = this.model.getConditionByName('territory_filter');
this.model.setCondition(condition, newValue);
this.model.activateCondition(condition);

this.model.updateData(
    function() { /* success — rows are now fresh */ },
    function(error) { /* error handling */ }
);
```

### Skuid Event Publishing

```javascript
// Notify other page components of state changes
skuid.events.publish('user_pref_updated', {
    clinicId: currentClinicId,
    timestamp: Date.now()
});
```

### Popup Integration

PM auto-detects and hides Skuid "Show Popup" chrome (jQuery UI dialog) on init, then implements custom dragging on headers:

```javascript
// Hide dialog title bar and padding
dialog.find('> .ui-dialog-titlebar').hide();
dialogContent.css({ padding: 0, background: 'transparent', border: 'none' });

// Implement custom drag on component headers
document.addEventListener('mousedown', function(e) {
    var header = e.target.closest('.pm-header, .pm-manage-header');
    if (!header) return;
    isDragging = true;
    // ... track mouse delta, update dialog position
});
```

---

## 8. Permission Model (PM)

### Role Resolution

User's Salesforce role/profile name is mapped to an internal role ID:

```javascript
var ROLE_MAP = {
    'Front Office': 'front_desk',
    'Front Desk': 'front_desk',
    'Clinic Admin': 'clinic_director',
    'Clinic Director': 'clinic_director',
    'System Administrator': 'all',
    'Chatter Clinical': 'clinician'
    // ... with case-insensitive and partial-match fallbacks
};
```

### Template Edit/Delete Permissions

```
System templates:  Sys Admin always; Clinic Director only if ALLOW_DIRECTOR_EDIT_SYSTEM flag = true
Shared/Global:     Sys Admin + Clinic Director
Personal (own):    Owner only
Personal (other):  Nobody
System templates:  Never deletable in UI (by anyone)
```

### Feature Flag

```javascript
// Temporary deployment flag — lets Clinic Directors edit system templates
// during initial setup. Set to false once templates are finalized.
var ALLOW_DIRECTOR_EDIT_SYSTEM = true;
```

---

## 9. For AI Assistants — Building New Components

When generating a new ONE Skuid component, follow these behavioral patterns:

1. **Structure**: Constructor function assigned to `comp.componentName`. Use prototype methods. Store state as `this.*` properties.

2. **Rendering**: Full `innerHTML` rebuild on each `render()`. No virtual DOM. Attach event listeners immediately after setting innerHTML. Preserve focus/cursor for search inputs via `setTimeout(fn, 0)`.

3. **Icons**: Copy needed SVGs from the shared set above. Use consistent sizing (14px for actions, 16–18px for headers). Store in a top-level `ICONS` object.

4. **Discipline colors**: Look up from `DISCIPLINE_COLORS` by Salesforce picklist value. Apply as inline styles, not CSS classes. Always provide a `DEFAULT_DISCIPLINE_COLOR` fallback.

5. **Loading states**: Use boolean flags (`isLoading`, `isSaving`). Show `ICONS.loader` (with spinner class) + descriptive text. Disable interactive elements during loading.

6. **Confirmation flows**: Always use an overlay + dialog. Never auto-proceed on destructive actions. For save operations, show the spinner *inside* the action button.

7. **Feedback**: Use `skuid.$.blockUI({ message, timeout })` for transient feedback. 1500–2000ms for success, 3000–4000ms for errors. Style errors with red border/background.

8. **Unsaved changes**: Snapshot state on load with `JSON.parse(JSON.stringify())`. Compare on every mutation. Gate navigation/clinic-switching behind a 3-option modal (Cancel / Discard / Save).

9. **Dropdowns**: Toggle `display: none/block`. Stop propagation on trigger click. Close all menus on document click. Store bound handler for cleanup.

10. **Cleanup**: Implement a `destroy()` method that removes all document-level event listeners.

---

## 10. State Class Cross-Reference

The CSS definitions for all JS-driven state classes documented in this file now live in **Section 8** of `one-design-system.css`. The quick-reference tables are in `ONE-DESIGN-SYSTEM.md` under "JS-Driven State Classes."

**44 state classes total** — 12 CPS, 32 PM — covering drag-and-drop, selection, send flow, token resolution, template management, editor toggles, and opt-in states. Each CSS rule includes a comment identifying the JS trigger that toggles it.
