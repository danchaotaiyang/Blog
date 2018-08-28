
首先需要  webpack  与  webpack-cli

	const path = require('path');
	module.exports = {

		// 入口文件
		entry: './src/main.js',

		// 输出文件
		output: {

			// 输出路径的绝对路径
			path: 'path.resolve(__dirname, 'dist')',

			// 输出的文件名
			filename: 'bundle.js'
		}
	};


入口和出口是最基本的配置，使用 `npx webpack` 进行打包。

默认自动压缩，其中有两种模式，一种开发模式，一种生产模式。

	npx webpack --mode development

	npx webpack --mode production

开发服务器

	const path = require('path');
	module.exports = {
		entry: './src/main.js',
		output: {
			path: 'path.resolve(__dirname, 'dist')',
			filename: 'bundle.js'
		},

		// 配置开发服务器
		devServer: {

			// 开发服务器内容地址
			contentBase: './dist',

			// 开发服务器host地址
			host: 'localhost',

			// 开发服务器端口
			port: '8081'
		}
	};

加载器

	const path = require('path');
	module.exports = {
		entry: './src/main.js',
		output: {
			path: 'path.resolve(__dirname, 'dist')',
			filename: 'bundle.js'
		},
		devServer: {
			contentBase: './dist',
			host: 'localhost',
			port: '8081'
		},

		// 加载器
		module: {
			rules: [
				{
					test: /\.css$/,
					use: ['style-loader', 'css-loader']
				}
			]
		}
	};

插件

	const path = require('path');
	module.exports = {
		entry: './src/main.js',
		output: {
			path: 'path.resolve(__dirname, 'dist')',
			filename: 'bundle.js'
		},
		devServer: {
			contentBase: './dist',
			host: 'localhost',
			port: '8081'
		}

		// 开发环境插件
		plugins: [
			// ……
		]
	};

常用插件

html-webpack-plugin

	plugins: [
		new HtmlWebpackPlugin({

			// 模板文件
			template: '',
	
			// 输出的文件名
			filename: '',

			// 压缩项
			minify: {

				// 移除引号
				removeAttributeQuotes: true
			},
			
			// 静态资源增加hash参数
			hash: true
		})
	]