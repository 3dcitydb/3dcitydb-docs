# Change Log

### 1.0.0 - Active

##### UPDATE
* Line breaks for long table rows are now added automatically, 
so that a horizontal scrollbar is no longer needed, 
see [`16e9b73`](https://github.com/3dcitydb/3dcitydb-docs/commit/16e9b735dc7f707de97339eadb6f1292155a7327).

##### FIXES
* Fixed tables that are too long to fix in one page and added autowrapping for table rows, 
see [`23414ad`](https://github.com/3dcitydb/3dcitydb-docs/commit/23414adfba8dd35cff3ec6d285926dc72c58fadd).
* Unicodes such as `≤` are now displayed correctly. 
The LaTeX engine was changed to xelatex.
Build with command `sphinx-build -M latexpdf source build/latex`, 
see [`f46330b`](https://github.com/3dcitydb/3dcitydb-docs/commit/f46330bf953ec63fb664dc03cf6f369d1a8a0792).  