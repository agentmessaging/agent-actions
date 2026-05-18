# 2. Bridge Script

The bridge MUST be injected into canvas HTML by the provider. It provides a single global function:

```javascript
window.maestro = {
  send: function(action, element, data) {
    window.parent.postMessage({
      type: 'canvas:interaction',
      action: action,
      element: element || null,
      data: data || null
    }, '*');
  }
};
```

Canvas authors use `maestro.send(action, element, data)` — never raw `postMessage`.

## Example Usage

```html
<button onclick="maestro.send('click', 'approve-btn', { approved: true })">
  Approve
</button>

<form onsubmit="event.preventDefault(); maestro.send('submit', 'feedback-form', {
  rating: document.getElementById('rating').value,
  comment: document.getElementById('comment').value
})">
  <input id="rating" type="number" min="1" max="5" />
  <textarea id="comment"></textarea>
  <button type="submit">Submit</button>
</form>
```
