[Medium](https://adityawiguna.medium.com/connect-mysql-database-from-electron-application-f050be5dda26)

When developing desktop application maybe sometime best database we can use is sqlite, because it simple and also easy to implement, in my case found one problem where sqlite is unstable.

For the information now i’m developing the application using MacOs and also windows, in my MacOs sqlite running very well but on my windows laptop they can showing error message like.

windows sqlite crashpad_client_win.cc(844)

based on this case, i decide to change my database to mysql for current project development, i installing XAMPP or Laragon on my client device and also as mysql server, let’s to step by step by step how to implement it.

For infomation for my project i use vite-electron-react by following [https://electron-vite.org/guide/](https://electron-vite.org/guide/)

1. **Install MySQL pluggin**

To use mysql sql on we can use mysql2 plugin and install only by one command

npm install --save mysql2  
  
// typescript  
npm install --save-dev @types/node

**2. Configure Database Connection**

create one file in electron/database.config.ts

export default {  
  HOST: "localhost",  
  USER: "root",  
  PASSWORD: undefined,  
  DATABASE: "database",  
};

**3. Add New IPC**

create your database ipc file on electron/database.ts

import { app, dialog, ipcMain } from "electron";  
  
import databaseConfig from "./database.config";  
import mysql from "mysql2";  
  
export const setupDatabase = () => {  
  const connection = mysql.createConnection({  
    host: databaseConfig.HOST,  
    user: databaseConfig.USER,  
    password: databaseConfig.PASSWORD,  
    database: databaseConfig.DATABASE,  
  });  
  
  connection.connect((error) => {  
    if (error) {  
      console.error("Database connection failed: ", error);  
      dialog.showErrorBox("Database connection failed", error.message);  
      app.quit();  
    } else {  
      console.log("Database connected successfully");  
    }  
  });  
  
  ipcMain.on(  
    "execute-query",  
    async (event, query: string, values: unknown[], requestId: string) => {  
      connection.query(query, values, (error, results) => {  
        if (error) {  
          event.reply(requestId, {  
            error: error.message || "Unknown error",  
          });  
        }  
        event.reply(requestId, results);  
      });  
    }  
  );  
};

after you create custom database.ts, now you must connected database.ts to your main electron code, by adding this line

import { setupDatabase } from "./database";  
setupDatabase();

or maybe full code will be like this

import { BrowserWindow, app } from "electron";  
  
import path from "node:path";  
import { setupDatabase } from "./database";  
  
setupDatabase();  
  
process.env.DIST = path.join(__dirname, "../dist");  
process.env.VITE_PUBLIC = app.isPackaged  
  ? process.env.DIST  
  : path.join(process.env.DIST, "../public");  
  
let win: BrowserWindow | null;  
// 🚧 Use ['ENV_NAME'] avoid vite:define plugin - Vite@2.x  
const VITE_DEV_SERVER_URL = process.env["VITE_DEV_SERVER_URL"];  
  
function createWindow() {  
  win = new BrowserWindow({  
    icon: path.join(process.env.VITE_PUBLIC, "electron-vite.svg"),  
    webPreferences: {  
      preload: path.join(__dirname, "preload.js"),  
      nodeIntegration: true,  
    },  
  });  
  
  win.webContents.openDevTools();  
  // console.log(win.webContents.getPrinters())  
  
  // Test active push message to Renderer-process.  
  win.webContents.on("did-finish-load", () => {  
    win?.webContents.send("main-process-message", new Date().toLocaleString());  
  });  
  
  if (VITE_DEV_SERVER_URL) {  
    win.loadURL(VITE_DEV_SERVER_URL);  
  } else {  
    // win.loadFile('dist/index.html')  
    win.loadFile(path.join(process.env.DIST, "index.html"));  
  }  
  win.setMinimumSize(400, 300);  
  win.maximize();  
}  
  
// Quit when all windows are closed, except on macOS. There, it's common  
// for applications and their menu bar to stay active until the user quits  
// explicitly with Cmd + Q.  
app.on("window-all-closed", () => {  
  if (process.platform !== "darwin") {  
    app.quit();  
    win = null;  
  }  
});  
  
app.on("activate", () => {  
  // On OS X it's common to re-create a window in the app when the  
  // dock icon is clicked and there are no other windows open.  
  if (BrowserWindow.getAllWindows().length === 0) {  
    createWindow();  
  }  
});  
  
app.whenReady().then(createWindow);

**4. Create Main Service To Call Database In Client File**

Now we must make helpers/service/utils file what ever you want, where it will be your bridge to call database from client, by following my sample code

import { v4 as uuidv4 } from "uuid";  
  
export const MySQL = {  
  runQuery: async <T>(query: string, values?: unknown[]): Promise<T> => {  
    const requestId: string = uuidv4();  
    return new Promise<T>((resolve, reject) => {  
      window.ipcRenderer.once(requestId, (_, result) => {  
        if (result.error) {  
          reject(new Error(result.error));  
        } else {  
          resolve(result as T);  
        }  
      });  
      window.ipcRenderer.send("execute-query", query, values, requestId);  
    });  
  },  
};

by this code i use uuid as requestId or on electron also call messageID, why i use uuid ?

i use uuid for avoiding conflict response when run multiple request to database, for the illustration like this

![](https://miro.medium.com/v2/resize:fit:700/1*GuX8uxUzcF73oBJJ7NoYiw.png)

Illustrating IPC Main Request and Reply

by this case uuid is best solution i found

**5. Call In Your Client File**

for take the data in client page, i make service file on page directory, this code like

import { MySQL } from "@/utils/mysqlUtils";  
import { ResultSetHeader } from "mysql2";  
  
export type Category = {  
  id?: number;  
  name: string;  
};  
  
export const CategoryService = {  
  getCategories: async (): Promise<Category[]> => {  
    const res = await MySQL.runQuery<Category[]>(`SELECT * FROM categories`);  
    return res;  
  },  
};

and call in your .tsx file like

CategoryService.getCategories()  
      .then((result) => {  
        setCategories(result);  
        setLoading(false);  
      })  
      .catch((error) => console.log(error));

you can custom then and catch result by your case.

Thank you for your interest i hope this will help you stuck on electron 😁

If you are enjoy and helpful with article, you can [Buy Me A Coffee](https://www.buymeacoffee.com/adityawiguna) to support me create another content, Thank You