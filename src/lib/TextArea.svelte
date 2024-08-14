<!-- Textarea.svelte -->
<script lang="ts">
  export let className = 'erise_text_area1';

  </script>
  
  <div
  class={`editor ${className}`}
    contenteditable="true"
    placeholder="Type here..."
    {...$$restProps} 

  >
  </div>
  
  <style>
    .editor {
      min-height: 200px;
      width: 100%;
      height: 100%;
      outline: none;
      border: 1px solid #ccc;
      padding: 10px;
      margin-bottom: 10px;
    }
  </style>
  