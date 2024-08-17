<script lang="ts">
	import { onMount } from 'svelte';

	export let className = 'erise_text_area1';

	onMount(() => {
		// Select all text areas with the given className
		const textareas = document.querySelectorAll('.' + className) as NodeListOf<HTMLElement>;

		textareas.forEach((textarea) => {
			// Assign the textarea element
			const editorContent = textarea;

			// Add click event listener to editorContent
			editorContent.addEventListener('click', updateActiveButtons);

			// Add keyboard shortcuts for undo, redo, and other formatting directly on editorContent
			editorContent.addEventListener('keydown', (event) => {
				if (event.ctrlKey) {
					switch (event.key.toLowerCase()) {
						case 'z':
							event.preventDefault();
							undo(editorContent);
							break;
						case 'y':
							event.preventDefault();
							redo(editorContent);
							break;
						case 'b':
							event.preventDefault();
							applyStyle('bold', editorContent);
							break;
						case 'i':
							event.preventDefault();
							applyStyle('italic', editorContent);
							break;
						case 'u':
							event.preventDefault();
							applyStyle('underline', editorContent);
							break;
					}
				}
			});
		});
	});

	function applyStyle(style: string, editorContent: HTMLElement, value: string = '') {
		if (editorContent) {
			document.execCommand(style, false, value);
			updateActiveButtons(); // Update button styles immediately
		}
	}

	function resetStyles(editorContent: HTMLElement) {
		if (editorContent) {
			const plainText = editorContent.innerText || editorContent.textContent;
			editorContent.innerHTML = plainText;
			editorContent.style.textAlign = 'left'; // Reset alignment to default (left)
			updateActiveButtons(); // Update button styles immediately
		}
	}

	function getContent(editorContent: HTMLElement) {
		if (editorContent) {
			alert(editorContent.innerHTML);
		}
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
		const fontFamilySelect = document.querySelector('select[data-font-family]') as HTMLSelectElement;
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

	function applyAlignment(alignment: string, editorContent: HTMLElement) {
		if (editorContent) {
			editorContent.style.textAlign = alignment;
			updateActiveButtons(); // Update button styles immediately
		}
	}

	function applyFontFamily(fontFamily: string, editorContent: HTMLElement) {
		if (editorContent) {
			document.execCommand('fontName', false, fontFamily);
			updateActiveButtons(); // Update button styles immediately
		}
	}

	function applyFontSize(fontSize: string, editorContent: HTMLElement) {
		if (editorContent) {
			document.execCommand('fontSize', false, fontSize);
			updateActiveButtons(); // Update button styles immediately
		}
	}

	function undo(editorContent: HTMLElement) {
		if (editorContent) {
			document.execCommand('undo');
		}
	}

	function redo(editorContent: HTMLElement) {
		if (editorContent) {
			document.execCommand('redo');
		}
	}
</script>

<!-- Toolbar and styles remain the same -->
<link
	rel="stylesheet"
	href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css"
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
		<button on:click={resetStyles}>
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
		</select>
	</div>

	<!-- 
		Replace the below commented code with your dynamic textareas.
		Each textarea needs to have the `className` (e.g., 'erise_text_area1') 
		class applied for the script to detect and apply the functionality.
	-->
	<!-- <div class="editor-container">
		<div
			class="editor erise_text_area1"
			contenteditable="true"
			placeholder="Type here..."
		></div>
	</div> -->
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
