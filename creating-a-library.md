## Angular Library

Create a new workspace for the development with a real angular application

- `ng new high-components --create-application=false`

After that let's create a library indeed

- `ng g library high-components --prefix=hc`

### Creating a new component

Now let's create a new component for our library

- `ng g c stories --project=high-components`

After that we need to serve the component in it's module's exports area

```ts
@NgModule({
 imports: [
 ],
 declarations: [
   StoriesComponent,
 ],
 exports: [
   StoriesComponent,
 ]
})
```

And finally you need to add a new entry for the component in the `public-api.ts``

```ts
export * from "./lib/stories/stories.component";
```

### Building and Packaging

In your root `package.json` you must add the build script

```json
"build_lib": "ng build high-component"
```

Let's pack our lib, create the following scripts in your root `package.json`

```json
"build:lib": "ng build high-components",
"npm:pack": "cd dist/high-components && npm pack",
"package": "npm run build:lib && npm run npm:pack"
```

Running `npm run package` it will build the dist and generate the `.tgz` file


### Publishing to NPM

- `npm login`

Now publish your generated `.tgz`

- `npm publish ./dist/high-components/high-components-0.0.1.tgz`