<script>
    import TextEditor from '$lib/TextEditor.svelte';
    import TextArea from '$lib/TextArea.svelte';
    import Toolbar from '$lib/Toolbar.svelte';
  </script>
  
  <main>    
    <!-- <TextEditor /> -->
    <Toolbar className="text-area3" />
    <TextArea className="text-area3"/>
    <div class="text-area3" contenteditable="true"> Hello there</div>
  </main>
  
  <style>
    .text-area3{
      border: 1px solid #ccc;
      padding: 10px;
      margin: 10px;
      border-radius: 5px;
      min-height: 200px;
    }
  </style>