# typeorm-esm
node sqlite esm typeorm

```javascript
import "reflect-metadata";
import { join, dirname } from "path";
import { fileURLToPath } from "url";
import { EntitySchema, DataSource } from "typeorm";

const __dirname = dirname(fileURLToPath(import.meta.url));
const dbfile = join(__dirname, "database.db");
class Post {
  constructor(id, title, text, categories) {
    this.id = id;
    this.title = title;
    this.text = text;
    this.categories = categories;
  }
}

const PostSchema = new EntitySchema({
  name: "Post",
  target: Post,
  columns: {
    id: {
      primary: true,
      type: "int",
      generated: true,
    },
    title: {
      type: "varchar",
    },
    text: {
      type: "text",
    },
  },
});
const dataSource = new DataSource({
  type: "sqlite",
  database: dbfile,
  synchronize: true,
  logging: false,
  entities: [PostSchema],
});

dataSource.initialize().then(async function (connection) {
  await connection.manager.save(new Post(5, 2, 3));

  // let post = new Post(2, 2, 3);
  // let postRepository = connection.getRepository(Post);
  // postRepository
  //   .save(post)
  //   .then((post) => console.log("Post has been saved: ", post))
  //   .catch((error) => console.log("Cannot save. Error: ", error));
});

```
