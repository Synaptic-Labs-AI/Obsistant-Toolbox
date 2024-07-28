```js
class CurrentNoteManager {
  constructor(app) {
    this.app = app;
  }

  async getCurrentNote() {
    const leaves = this.app.workspace.getLeavesOfType("markdown");
    const activeLeaf = leaves
      .filter(leaf => leaf.activeTime > 0)
      .sort((a, b) => b.activeTime - a.activeTime)[0];
    
    if (activeLeaf && activeLeaf.view && activeLeaf.view.file) {
      return activeLeaf.view.file;
    }

    // Fallback to the most recent markdown file
    const files = this.app.vault.getMarkdownFiles();
    if (files.length === 0) {
      throw new Error("No Markdown files found in the vault.");
    }
    return files.sort((a, b) => b.stat.mtime - a.stat.mtime)[0];
  }

  async getNoteContent(file) {
    try {
      return await this.app.vault.read(file);
    } catch (error) {
      console.error("Error reading file:", error);
      throw new Error(`Error reading the file: ${error.message}`);
    }
  }

  async getCurrentNoteContent() {
    const currentNote = await this.getCurrentNote();
    return await this.getNoteContent(currentNote);
  }

  getFileName(file) {
    return file.name;
  }
/* Needs to use something else, this isn't right
  async getFrontmatter(file) {
    const content = await this.getNoteContent(file);
    const frontmatterRegex = /^---\s*\n([\s\S]*?)\n---\s*\n/;
    const match = content.match(frontmatterRegex);
    if (!match) return null;

    try {
      return this.app.metadataCache.parseFrontMatter(match[1]);
    } catch (error) {
      console.error("Error parsing frontmatter:", error);
      return null;
    }
  }
*/
  async getTags(file) {
    const cache = this.app.metadataCache.getFileCache(file);
    return cache?.tags?.map(tag => tag.tag) || [];
  }

  async getLinks(file) {
    const cache = this.app.metadataCache.getFileCache(file);
    return cache?.links?.map(link => link.link) || [];
  }

  async getHeadings(file) {
    const cache = this.app.metadataCache.getFileCache(file);
    return cache?.headings?.map(heading => ({
      level: heading.level,
      text: heading.heading
    })) || [];
  }

  async updateNoteContent(file, newContent) {
    try {
      await this.app.vault.modify(file, newContent);
    } catch (error) {
      console.error("Error updating file:", error);
      throw new Error(`Error updating the file: ${error.message}`);
    }
  }

  async createNote(folderPath, fileName, content = "") {
    try {
      const path = `${folderPath}/${fileName}`;
      return await this.app.vault.create(path, content);
    } catch (error) {
      console.error("Error creating file:", error);
      throw new Error(`Error creating the file: ${error.message}`);
    }
  }

  async deleteNote(file) {
    try {
      await this.app.vault.delete(file);
    } catch (error) {
      console.error("Error deleting file:", error);
      throw new Error(`Error deleting the file: ${error.message}`);
    }
  }

  async renameNote(file, newName) {
    try {
      await this.app.fileManager.renameFile(file, `${file.parent.path}/${newName}`);
    } catch (error) {
      console.error("Error renaming file:", error);
      throw new Error(`Error renaming the file: ${error.message}`);
    }
  }

  async moveNote(file, newPath) {
    try {
      await this.app.fileManager.renameFile(file, `${newPath}/${file.name}`);
    } catch (error) {
      console.error("Error moving file:", error);
      throw new Error(`Error moving the file: ${error.message}`);
    }
  }

  async searchInNote(file, searchTerm) {
    const content = await this.getNoteContent(file);
    const lines = content.split('\n');
    return lines.reduce((results, line, index) => {
      if (line.toLowerCase().includes(searchTerm.toLowerCase())) {
        results.push({ line: index + 1, content: line.trim() });
      }
      return results;
    }, []);
  }

  async getBacklinks(file) {
    const backlinkMap = this.app.metadataCache.getBacklinksForFile(file)?.data;
    return backlinkMap ? Object.keys(backlinkMap) : [];
  }

  async getNoteMetadata(file) {
    return {
      name: this.getFileName(file),
      path: file.path,
      size: file.stat.size,
      created: file.stat.ctime,
      modified: file.stat.mtime,
      tags: await this.getTags(file),
      links: await this.getLinks(file),
      headings: await this.getHeadings(file),
      backlinks: await this.getBacklinks(file)
    };
  }
}
```