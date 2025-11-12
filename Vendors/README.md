# Vendors

This folder contains third-party submodules or dependencies.

## Usage 

### Adding a New Submodule

To add a new submodule:

1. Run the following command in the repository root:

```
git submodule add <repository-url> Vendors/<FolderName>
```

2. Commit the change:

```
git commit -m "Add ExampleLibrary submodule"
```

3. Push the update:

```
git push
```


### Update Submodules

```
git submodule update --init --recursive
```

## Notes
- Use Git submodules for third-party code.
- Keep each vendor library in a separate folder.
- Copy to PackageLocalDirectory for newly update.
