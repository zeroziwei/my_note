- [ ] coc 快捷键整理

| 快捷键               | 功能                                               |
| ----------------- | ------------------------------------------------ |
| C-g>u             | breaks current undo, please make your own choice |
| c-j>              | trigger snippets                                 |
| c-o>              | trigger completion                               |
| `[g` and `]g`     | navigate diagnostics                             |
| `:CocDiagnostics` | diagnostics of current buffer in location list   |
| gd                | go to definition                                 |
| gy                | go to type-definition                            |
| gi                | go to implementation                             |
| gr                | go to references                                 |
| K                 | show documentation in preview window             |
| rn                | Symbol renaming                                  |
| `<leader>f`       | Formatting selected code                         |
|                   |                                                  |





-- Update signature help on jump placeholder

-- Apply codeAction to the selected region
-- Example: `<leader>aap` for current paragraph
local opts = { silent = true, nowait = true }
keyset("x", "<leader>a", "<Plug>(coc-codeaction-selected)", opts)
keyset("n", "<leader>a", "<Plug>(coc-codeaction-selected)", opts)

-- Remap keys for apply code actions at the cursor position.
keyset("n", "<leader>ac", "<Plug>(coc-codeaction-cursor)", opts)
-- Remap keys for apply source code actions for current file.
keyset("n", "<leader>as", "<Plug>(coc-codeaction-source)", opts)
-- Apply the most preferred quickfix action on the current line.
keyset("n", "<leader>qf", "<Plug>(coc-fix-current)", opts)

-- Remap keys for apply refactor code actions.
keyset("n", "<leader>re", "<Plug>(coc-codeaction-refactor)", { silent = true })
keyset("x", "<leader>r", "<Plug>(coc-codeaction-refactor-selected)", { silent = true })
keyset("n", "<leader>r", "<Plug>(coc-codeaction-refactor-selected)", { silent = true })

-- Run the Code Lens actions on the current line
keyset("n", "<leader>cl", "<Plug>(coc-codelens-action)", opts)


-- Map function and class text objects
-- NOTE: Requires 'textDocument.documentSymbol' support from the language server
keyset("x", "if", "<Plug>(coc-funcobj-i)", opts)
keyset("o", "if", "<Plug>(coc-funcobj-i)", opts)
keyset("x", "af", "<Plug>(coc-funcobj-a)", opts)
keyset("o", "af", "<Plug>(coc-funcobj-a)", opts)
keyset("x", "ic", "<Plug>(coc-classobj-i)", opts)
keyset("o", "ic", "<Plug>(coc-classobj-i)", opts)
keyset("x", "ac", "<Plug>(coc-classobj-a)", opts)
keyset("o", "ac", "<Plug>(coc-classobj-a)", opts)


-- Remap <C-f> and <C-b> to scroll float windows/popups
---@diagnostic disable-next-line: redefined-local
local opts = { silent = true, nowait = true, expr = true }
keyset("n", "<C-f>", 'coc#float#has_scroll() ? coc#float#scroll(1) : "<C-f>"', opts)
keyset("n", "<C-b>", 'coc#float#has_scroll() ? coc#float#scroll(0) : "<C-b>"', opts)
keyset("i", "<C-f>",
	'coc#float#has_scroll() ? "<c-r>=coc#float#scroll(1)<cr>" : "<Right>"', opts)
keyset("i", "<C-b>",
	'coc#float#has_scroll() ? "<c-r>=coc#float#scroll(0)<cr>" : "<Left>"', opts)
keyset("v", "<C-f>", 'coc#float#has_scroll() ? coc#float#scroll(1) : "<C-f>"', opts)
keyset("v", "<C-b>", 'coc#float#has_scroll() ? coc#float#scroll(0) : "<C-b>"', opts)


-- Use CTRL-S for selections ranges
-- Requires 'textDocument/selectionRange' support of language server
keyset("n", "<C-s>", "<Plug>(coc-range-select)", { silent = true })
keyset("x", "<C-s>", "<Plug>(coc-range-select)", { silent = true })


-- Add `:Format` command to format current buffer
vim.api.nvim_create_user_command("Format", "call CocAction('format')", {})

-- " Add `:Fold` command to fold current buffer
vim.api.nvim_create_user_command("Fold", "call CocAction('fold', <f-args>)", { nargs = '?' })

-- Add `:OR` command for organize imports of the current buffer
vim.api.nvim_create_user_command("OR", "call CocActionAsync('runCommand', 'editor.action.organizeImport')", {})

-- Add (Neo)Vim's native statusline support
-- NOTE: Please see `:h coc-status` for integrations with external plugins that
-- provide custom statusline: lightline.vim, vim-airline
vim.opt.statusline:prepend("%{coc#status()}%{get(b:,'coc_current_function','')}")

-- -- Mappings for CoCList
-- -- code actions and coc stuff
-- ---@diagnostic disable-next-line: redefined-local
-- local opts = {silent = true, nowait = true}
-- -- Show all diagnostics
-- keyset("n", "<space>a", ":<C-u>CocList diagnostics<cr>", opts)
-- -- Manage extensions
-- keyset("n", "<space>e", ":<C-u>CocList extensions<cr>", opts)
-- -- Show commands
-- keyset("n", "<space>c", ":<C-u>CocList commands<cr>", opts)
-- -- Find symbol of current document
-- keyset("n", "<space>o", ":<C-u>CocList outline<cr>", opts)
-- -- Search workspace symbols
-- keyset("n", "<space>s", ":<C-u>CocList -I symbols<cr>", opts)
-- -- Do default action for next item
-- keyset("n", "<space>j", ":<C-u>CocNext<cr>", opts)
-- -- Do default action for previous item
-- keyset("n", "<space>k", ":<C-u>CocPrev<cr>", opts)
-- -- Resume latest coc list
-- keyset("n", "<space>p", ":<C-u>CocListResume<cr>", opts)