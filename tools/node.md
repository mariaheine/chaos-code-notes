
- `npm ls webpack-dev-server`
	- will tell you which version of **webpack-dev-server** you have
	- you can also just peek into **package.json**

## basic commands

- `npm install packagename`
- `npm uninstall packagename`
- `npm update`
	- this will update to the latest major/minor versions while respecting your `^` semver range
	- if you want to update a package past a major release you will need to reinstall it, for example like:
		- `npm install @tiptap/core@latest`
## audit

> It is a thing you should be doing while working with node regularly!

> Consider adding `npm audit --audit-level=high` as a *precommit hook*.

- `npm audit`
	- this will do nothing yet, it will tell you what it could fix
- `npm audit --json`       
	- JSON output for detailed analysis
	- `npm audit --json | jq '.metadata.vulnerabilities'`
		- this will neatly print a count of all vulnerabilities by severity
		- [ ] what is this jq thing
- `npm audit --audit-level=high`
	- Only show high/critical issues
- `npm audit fix --dry-run`
	- See what would be fixed
- `npm audit fix`
	- attempt fixing things automatically
	- you can add `--force` flag to make it more aggressive (might break things)