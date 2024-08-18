<script lang="ts">
	import { onMount } from 'svelte';

	export let className = 'erise_text_area1';
	let textareas: NodeListOf<HTMLElement>;
	let editor: HTMLElement;
	onMount(() => {
		// Select all text areas with the given className
		textareas = document.querySelectorAll('.' + className) as NodeListOf<HTMLElement>;

		textareas.forEach((textarea) => {
			// Add click event listener to update the global editorContent and updateActiveButtons
			textarea.addEventListener('click', (event) => {
				editor = event.currentTarget as HTMLElement;
				updateActiveButtons();
			});

			// Add keyboard shortcuts for undo, redo, and other formatting directly on editorContent
			// Add keyboard shortcuts
			textarea.addEventListener('keydown', (event) => handleEditorKeydown(event));
		});
	});

	function handleEditorKeydown(event: KeyboardEvent) {
		if (event.ctrlKey) {
			switch (event.key.toLowerCase()) {
				case 'z':
					event.preventDefault();
					undo();
					break;
				case 'y':
					event.preventDefault();
					redo();
					break;
				case 'b':
					event.preventDefault();
					applyStyle('bold');
					break;
				case 'i':
					event.preventDefault();
					applyStyle('italic');
					break;
				case 'u':
					event.preventDefault();
					applyStyle('underline');
					break;
			}
		}
	}

	// Function to check if the clicked element is inside a textarea
	function isClickInsideTextarea(event: MouseEvent): boolean {
		// Get the element that was clicked
		let element = event.target as HTMLElement;

		// Check if the clicked element or any of its parents is a textarea
		for (let i = 0; i < textareas.length; i++) {
			if (textareas[i].contains(element)) {
				return true; // The click occurred inside a textarea
			}
		}
		return false; // The click did not occur inside any textarea
	}

	function applyStyle(style: string, value: string = '') {
		document.execCommand(style, false, value);
		updateActiveButtons(); // Update button styles immediately
	}

	function updateActiveButtons() {
		const selection = window.getSelection();

		// Ensure the selection is within one of the editorContent elements
		if (!selection || !selection.anchorNode) return;

		const commands = ['bold', 'italic', 'underline'];
		commands.forEach((command) => {
			const button = document.querySelector(`button[data-command=${command}]`);
			if (document.queryCommandState(command)) {
				button?.classList.add('active');
			} else {
				button?.classList.remove('active');
			}
		});

		// Text alignment
		const alignments = ['left', 'center', 'right', 'justify'];
		alignments.forEach((align) => {
			const button = document.querySelector(`button[data-align=${align}]`);
			if ((selection.anchorNode?.parentNode as HTMLElement)?.style?.textAlign === align) {
				button?.classList.add('active');
			} else {
				button?.classList.remove('active');
			}
		});

		// Font family
		const fontFamilySelect = document.querySelector(
			'select[data-font-family]'
		) as HTMLSelectElement;
		if (fontFamilySelect) {
			const currentFont = document.queryCommandValue('fontName').replace(/"/g, '');
			fontFamilySelect.value = currentFont;
		}

		// Font size
		const fontSizeSelect = document.querySelector('select[data-font-size]') as HTMLSelectElement;
		if (fontSizeSelect) {
			const currentFontSize = document.queryCommandValue('fontSize');
			fontSizeSelect.value = currentFontSize;
		}
	}

	function applyAlignment(alignment: string) {
		
		editor.style.textAlign = alignment;
		updateActiveButtons(); // Update button styles immediately
	}

	function applyFontFamily(fontFamily: string) {
		

		document.execCommand('fontName', false, fontFamily);
		updateActiveButtons(); // Update button styles immediately
	}

	function applyFontSize(fontSize: string) {
		
		document.execCommand('fontSize', false, fontSize);
		updateActiveButtons(); // Update button styles immediately
	}

	function undo() {
		document.execCommand('undo');
	}

	function redo() {
		document.execCommand('redo');
	}

	function deleteParentIfStyle(parentElement: HTMLElement) {
		const styleTags = ['B', 'STRONG', 'I', 'EM', 'U', 'SPAN', 'UL', 'LI', 'OL']; // Add other tags if needed
		// Stop condition: If parentElement has contenteditable="true" or if it's not a style-related tag
		if (
			parentElement.getAttribute('contenteditable') === 'true' ||
			!styleTags.includes(parentElement.tagName)
		) {
			return; // Exit the function if either condition is met
		}

		// Check if the parent element is a style-related tag
		if (parentElement && styleTags.includes(parentElement.tagName)) {
			// If the parent element is a style-related tag, remove it but keep its children
			const grandparent = parentElement.parentElement;
			while (parentElement.firstChild) {
				grandparent.insertBefore(parentElement.firstChild, parentElement);
			}
			grandparent.removeChild(parentElement);
			deleteParentIfStyle(grandparent); // Recursively check the grandparent
		}
	}

	function resetSelectedStyle() {
		// Step 1: Get the current selection using window.getSelection().
		const selection = window.getSelection();
		if (selection?.rangeCount) {
			const range = selection.getRangeAt(0);

			// Step 2: Clone the selected content and place it in a temporary div.
			const content = range.cloneContents();
			let content_textcontent =
				range.cloneContents().innerText || range.cloneContents().textContent;

			// Step 3: Remove the original content
			range.deleteContents();

			// Step 4: Check if it is surrended by a style tag and remove it
			let parentElement = range.commonAncestorContainer.parentElement;

			if (parentElement.getAttribute('contenteditable') === 'true') {
				// If parent is the original div, replace its content with plain text
				const plainText = parentElement.innerText || parentElement.textContent;
				parentElement.innerHTML = plainText;
			} else {
				// Function to check and delete the parent if it has certain styles
				deleteParentIfStyle(parentElement);
			}

			// Step 5: Remove the original content and replace it with plain text.
			range.deleteContents();
			range.insertNode(document.createTextNode(content_textcontent));

			// Step 6: Optionally, clean up the selection.
			selection.removeAllRanges();
		}
		applyAlignment('left')
		updateActiveButtons();
	}
</script>

<!-- Toolbar and styles remain the same -->
<link
	rel="stylesheet"
	href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css"
/>
<link
	href="https://fonts.googleapis.com/css2?family=Amatic+SC&family=Baloo+2:wght@400;500;700&family=Caveat&family=Fredoka+One&family=Gaegu&family=Pangolin&family=Quicksand:wght@400;500;600&family=Raleway:wght@400;500;600&display=swap"
	rel="stylesheet"
/>

<div>
	<div class="toolbar">
		<button data-command="bold" on:click={() => applyStyle('bold')}>
			<i class="fas fa-bold"></i>
		</button>
		<button data-command="italic" on:click={() => applyStyle('italic')}>
			<i class="fas fa-italic"></i>
		</button>
		<button data-command="underline" on:click={() => applyStyle('underline')}>
			<i class="fas fa-underline"></i>
		</button>
		<button on:click={() => applyStyle('insertOrderedList')}>
			<i class="fas fa-list-ol"></i>
		</button>
		<button on:click={() => applyStyle('insertUnorderedList')}>
			<i class="fas fa-list-ul"></i>
		</button>
		<button data-align="left" on:click={() => applyAlignment('left')}>
			<i class="fas fa-align-left"></i>
		</button>
		<button data-align="center" on:click={() => applyAlignment('center')}>
			<i class="fas fa-align-center"></i>
		</button>
		<button data-align="right" on:click={() => applyAlignment('right')}>
			<i class="fas fa-align-right"></i>
		</button>
		<button data-align="justify" on:click={() => applyAlignment('justify')}>
			<i class="fas fa-align-justify"></i>
		</button>
		<button on:click={resetSelectedStyle}>
			<i class="fa-solid fa-text-slash"></i>
		</button>
		<button on:click={undo}>
			<i class="fas fa-undo"></i>
		</button>
		<button on:click={redo}>
			<i class="fas fa-redo"></i>
		</button>
		<select data-font-size on:change={(event) => applyFontSize(event.target.value)}>
			<option value="1">10px</option>
			<option value="2">13px</option>
			<option value="3">16px</option>
			<option value="4">18px</option>
			<option value="5">24px</option>
			<option value="6">32px</option>
			<option value="7">48px</option>
		</select>
		<select data-font-family on:change={(event) => applyFontFamily(event.target.value)}>
			<option value="Arial">Arial</option>
			<option value="Courier New">Courier New</option>
			<option value="Georgia">Georgia</option>
			<option value="Times New Roman">Times New Roman</option>
			<option value="Verdana">Verdana</option>
			<option value="Amatic SC">Amatic SC</option>
			<option value="Baloo 2">Baloo 2</option>
			<option value="Caveat">Caveat</option>
			<option value="Fredoka One">Fredoka One</option>
			<option value="Gaegu">Gaegu</option>
			<option value="Pangolin">Pangolin</option>
			<option value="Quicksand">Quicksand</option>
			<option value="Raleway">Raleway</option>
		</select>
	</div>
</div>

<style>
	.editor-container {
		border: 1px solid #ccc;
		padding: 10px;
		margin-bottom: 10px;
	}

	.toolbar button,
	.toolbar select {
		margin-right: 5px;
		background: none;
		border: none;
		cursor: pointer;
		font-size: 16px;
	}

	.toolbar button i {
		font-size: 18px;
		color: #333;
		transition: color 0.3s ease; /* Smooth transition for hover effect */
	}
	.toolbar button {
		padding: 5px;
		border-radius: 5px;
		margin: 5px;
	}
	.toolbar button:hover {
		background-color: #75757533; /* Change color on hover */
	}

	.toolbar button.active i {
		color: #007bff; /* Change color for active buttons */
	}
	.editor {
		min-height: 200px;
		outline: none;
	}

	.toolbar select {
		padding: 2px;
		font-size: 16px;
	}
</style>
