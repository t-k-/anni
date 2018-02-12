<template>
<div>
	<h4>今天是{{today}}，宝宝和嘉嘉的重要日子：</h4>
	<ul>
		<li v-for="d in dates">
		{{fromNow(d.day)}}{{d.description}} ({{disp(d.day)}}){{postwords(d)}}</span>
		</li>
	</ul>

	<div class="imgarea">
		<img :src="giflink"/>
		<div class="signature">
		把嘉嘉放在宝宝<i class="fa fa-heart heart" aria-hidden="true"></i>上～
		</div>
	</div>
</div>
</template>

<script>
import moment from 'moment';
import datelist from './datelist.js';

export default {
	data: function () {
		return {
			"gifnum": 1,
			"dates": datelist.datelist
		};
	},
	created: function () {
		var vm = this;
		setInterval(function () {
			vm.changeGif();
		}, 10000);

		moment.locale('zh-cn');
	},
	computed: {
		today: function () {
			return moment().format('YY年MM月DD日');
		},
		giflink: function () {
			return 'img/' + this.gifnum + '.gif';
		}
	},
	methods: {
		processArg: function (arg) {
			var thisyear = moment().year();
			if (arg.length < 'xxxx-xx-xx'.length)
				return '' + thisyear + '-' + arg;
			else
				return arg;
		},
		fromNow: function (arg) {
			var datstr = this.processArg(arg);
			return moment(datstr).fromNow();
		},
		postwords: function (item) {
			var is_holiday = (item.day.length < 'xxxx-xx-xx'.length) ? true:false;
			var datstr = this.processArg(item.day);
			var thisyear = moment().year();
			var theday = moment(datstr).year(thisyear);
			var left = theday.diff(moment(), 'days');
			if (left == 0) {
				if (is_holiday)
					return '，祝地球对面的嘉嘉和威威' + item.description + '愉快！';
				else
					return '，周年愉快！';
			} else {
				if (is_holiday)
					return '。';
				if (left < 0)
					theday = moment(datstr).year(thisyear).add(1, 'y');
				return '，' + moment(theday).fromNow() + '周年纪念。';
			}
		},
		disp: function (arg) {
			var datstr = this.processArg(arg);
			if (arg.length < 'xxxx-xx-xx'.length)
				return moment(datstr).format('MM月DD日');
			else
				return moment(datstr).format('YY年MM月DD日');
		},
		changeGif: function () {
			console.log(this.giflink);
			this.gifnum = this.gifnum % 12 + 1;
		}
	}
}
</script>

<style>
.heart {
    color: #fc2e5a;
	margin-left: 2px;
	margin-right: 2px;
}
div.signature {
	color: #131413;
	text-align: right;
}
div.imgarea {
	display: inline-block;
}
li {
	color: grey;
	list-style-type: none;
}
</style>
