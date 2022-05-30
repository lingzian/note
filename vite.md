预构建 它们可以提高页面加载速度，并将 CommonJS / UMD 转换为 ESM 格式。预构建这一步由 esbuild 执行，这使得 Vite 的冷启动时间比任何基于 JavaScript 的打包器都要快得多。



在vite打包项目中 出现 Error [ERR_REQUIRE_ESM]: require() of ES Module 错误
是因为vite.config.ts是强制了commomjs规范，如果有引用纯esModuleD的包会报以上错误

