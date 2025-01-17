# zoning-guide-detroit

Let's build a website that helps explain Detroit's zoning code.

We started this project on the 2018-09-10 civic tech hackathon at WeWork. Our hackathon slide deck can be found [here](https://docs.google.com/presentation/d/1pZMpCYmuuGy8EJjk9DJUdPwjpkRc9JUTEUQBDbnh7XU/edit#slide=id.g6099c04ea9_2_0)!

This site uses the [Gatsby](https://www.gatsbyjs.org/tutorial/) static site framework along with [React](https://reactjs.org/tutorial/tutorial.html) to create site pages and components.

## Getting started

1. Install the Gatsby CLI: `npm install -g gatsby-cli`

2. Clone this repository: `git@github.com:Detroit-Urban-Tech-Meetup/zoning-guide-detroit.git`

3. Enter the directory: `cd zoning-guide-detroit`

4. Next, you `yarn` or `npm install`

5. Replace `<your API key>` with [your Airtable API Key](https://support.airtable.com/hc/en-us/articles/219046777-How-do-I-get-my-API-key-) in `example.env` and rename to `.env.development`

6. Start developing with `gatsby develop` - Gatsby will access the Airtable, build nodes, etc and will start serving at `localhost:8000`. You can access the GraphiQL Explorer at `localhost:8000/___graphql`: this is the internal store of all the data that Gatsby can use to build the site.

## Site structure

- We pull data from Airtable with the `gatsby-source-airtable` plugin; this is configured in `gatsby-config.js`. This data is stored in the internal GraphQL API that's accessible at `localhost:8000/___graphql`

- In the `gatsby-node.js` file, we query the internal GraphQL API and generate site pages with that query result.

- This site page points to the template file `src/templates/zone-page.js`: here, another GraphQL API call is made (this one uses the `$zone` variable)

- the React components used in the site are stored in `src/components`:

  - `layout.js` is the wrapper for all site pages
  - `header.js` is used in `layout.js`

- individual site pages that are not generated by `gatsby-node.js` live in `src/pages`:

  - `index.js` is what you see at the site's root, i.e. `localhost:8000`. It has its own GraphQL query as well.

## Contributing

### Uses (in Airtable)

This is the big initial data entry that needs to happen.

For each use, here's what needs to happen:

1. Find the use in the use table section of the zoning PDF. (This section is landscape-oriented, and starts on pg. 388)

2. Each use is a row; the columns represent the different zones.

3. Look at your use, and for each zone marked with a "R", enter that zone in the "By-right in" field in Airtable. Take advantage of the autocomplete feature; you should be able to type `M1`, quickly hit enter to add, and start typing the next zone. You can get pretty fast at this once you've done it a few times.

4. Do the same for the "Conditional in" field; for each zone marked with a "C", enter that zone in the "Conditional in" field in Airtable.

5. Do the same for the "Legislative approval in" field; for each zone marked with a "L", enter that zone in the "Legislative approval in" field in Airtable. (Typically, this is only the `PD` district)

(I like to do these above steps for a whole chunk of uses at a time).

6. Now, for each use, do a search in the PDF for the use name.

7. If you find "Use Regulations", copy and paste that text into the Airtable under "Use Regulations". *Be sure not to copy and paste text with line breaks - this will overwrite rows beneath!!* The macOS Preview app will automatically strip out line breaks; there is a hack you can use for Ubuntu/Windows outlined [here](https://superuser.com/a/796341) -- feel free to ping Jimmy for details about how to implement this, as it's really key to doing this work efficiently.

8 If you find the use has a definition, copy and paste that definition into the "Definitions" field in Airtable.