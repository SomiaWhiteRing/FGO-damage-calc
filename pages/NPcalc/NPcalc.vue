<template>
	<view class="wrap">
		<u-form :model="form" ref="uForm" @submit="formSubmit">
			<u-form-item label-width=auto label="英灵攻击力"><u-input placeholder="英灵的面板数据" type="number" v-model.number="form.ATK1" name="ATK1"/></u-form-item>
			<u-form-item label-width=auto label="礼装攻击力"><u-input placeholder="礼装的面板数据" type="number" v-model.number="form.ATK2" /></u-form-item>
			<u-form-item label-width=auto label="宝具倍率(%)"><u-input placeholder="根据宝具等级变动" type="number" v-model.number="form.MUT" /></u-form-item>
			<u-form-item label-width=auto label="宝具类型">
				<u-radio-group v-model="NPType">
					<u-radio v-for="(item, index) in NPTypelist" :key="index" :name="item.name" :disabled="item.disabled">
						{{ item.name }}
					</u-radio>
				</u-radio-group>
			</u-form-item>
			<u-form-item label-width=auto label="从者职阶">
				<u-radio-group v-model="Class">
					<u-radio v-for="(item, index) in Classlist" :key="index" :name="item.name" :disabled="item.disabled">
						{{ item.name }}
					</u-radio>
				</u-radio-group>
			</u-form-item>
			<u-form-item label-width=auto label="职阶相性">
				<u-radio-group v-model="RCType">
					<u-radio v-for="(item, index) in RCTypelist" :key="index" :name="item.name" :disabled="item.disabled">
						{{ item.name }}
					</u-radio>
				</u-radio-group>
			</u-form-item>
			<u-form-item label-width=auto label="隐藏属性">
				<u-radio-group v-model="HDType">
					<u-radio v-for="(item, index) in HDTypelist" :key="index" :name="item.name" :disabled="item.disabled">
						{{ item.name }}
					</u-radio>
				</u-radio-group>
			</u-form-item>
			<u-form-item label-width=auto label="攻击力提升(%)"><u-input placeholder="从者获得的攻击力类BUFF" type="number" v-model.number="form.BUFFATK" /></u-form-item>
			<u-form-item label-width=auto label="防御力提升(%)"><u-input placeholder="敌方持有的防御力类BUFF" type="number" v-model.number="form.BUFFDEF" /></u-form-item>
			<u-form-item label-width=auto label="指令卡性能提升(%)"><u-input placeholder="从者获得的色卡威力提升BUFF" type="number" v-model.number="form.BUFFCARD" /></u-form-item>
			<u-form-item label-width=auto label="指令卡耐性(%)"><u-input placeholder="敌方持有的色卡威力下降BUFF" type="number" v-model.number="form.BUFFUNCARD" /></u-form-item>
			<u-form-item label-width=auto label="特攻效果(%)"><u-input placeholder="从者获得的特攻类BUFF" type="number" v-model.number="form.BUFFSATK" /></u-form-item>
			<u-form-item label-width=auto label="特防效果(%)"><u-input placeholder="敌方持有的特防类BUFF" type="number" v-model.number="form.BUFFSDEF" /></u-form-item>
			<u-form-item label-width=auto label="宝威提升(%)"><u-input placeholder="从者获得的宝具威力类BUFF" type="number" v-model.number="form.BUFFNP" /></u-form-item>
			<u-form-item label-width=auto label="宝具特攻(%)"><u-input placeholder="宝具特有的特攻的数值(一般为100%)" type="number" v-model.number="form.BUFFSNP" /></u-form-item>
			<u-form-item label-width=auto label="伤害附加"><u-input placeholder="从者获得的伤害附加类BUFF" type="number" v-model.number="form.EXTRA" /></u-form-item>
			<u-form-item label-width=auto label="伤害减免"><u-input placeholder="敌方持有的伤害减免类BUFF" type="number" v-model.number="form.UNEXTRA" /></u-form-item>
			<u-button @click="setdefault">设为默认数据</u-button>
			<u-button @click="submit" form-type="submit">计算结果</u-button>
		</u-form>
		<u-modal v-model="show" title="计算结果">
			<view class="slot-content">
				<br>{{jieguo1}}<br>{{jieguo2}}
			</view>
		</u-modal>
		<u-modal v-model="set">
			<view class="slot-content">
				<rich-text :nodes="content"></rich-text>
			</view>
		</u-modal>
	</view>
</template>

<script>
export default {
	data() {
		return {
			show: false,
			set: false,
			jieguo1: '',
			jieguo2: '',
			form: {
				ATK1: '',
				ATK2: '',
				MUT: '',
				BUFFATK:'',
				BUFFDEF:'',
				BUFFCARD:'',
				BUFFUNCARD:'',
				BUFFSATK:'',
				BUFFSDEF:'',
				BUFFNP:'',
				BUFFSNP:'',
				EXTRA: '',
				UNEXTRA: '',
			},
			Classlist: [
				{name: 'Saber'},
				{name: 'Archer'},
				{name: 'Lancer'},
				{name: 'Rider'},
				{name: 'Caster'},
				{name: 'Assassin'},
				{name: 'Berserker'},
				{name: 'Ruler'},
				{name: 'Avenger'},
				{name: 'Alterego'},
				{name: 'MoonCancer'},
				{name: 'Foreigner'},
				{name: 'Shielder'},
			],
			Class: 'Saber',
			NPTypelist: [
				{name: 'Buster'},
				{name: 'Art'},
				{name: 'Quick'}
			],
			NPType: 'Buster',
			RCTypelist: [
				{name: '被克制'},
				{name: '无克制'},
				{name: '通常克制'},
				{name: '狂阶克制'}
			],
			RCType: '被克制',
			HDTypelist: [
				{name: '被克制'},
				{name: '无克制'},
				{name: '克制'}
			],
			HDType: '被克制',
			content:'已将数据设置为：<br>我方：阿尔托莉雅·潘德拉贡（saber）(宝具LV1)<br>敌方：骷髅兵（saber）<br>时的情况',
			switchVal: false,
		};
	},
	methods:{
		setdefault(){
			this.form.ATK1=12221
			this.form.ATK2=0
			this.form.MUT=400
			this.form.BUFFATK=0
			this.form.BUFFDEF=0
			this.form.BUFFCARD=0
			this.form.BUFFUNCARD=0
			this.form.BUFFSATK=0
			this.form.BUFFSDEF=0
			this.form.BUFFNP=0
			this.form.BUFFSNP=100
			this.form.EXTRA=0
			this.form.UNEXTRA=0
			this.Class="Saber"
			this.CARDType="Buster"
			this.RCType="无克制"
			this.HDType="克制"
			this.set=true
		},
		submit(){
			var NPTP=1.0;
			if (this.NPType == "Buster"){NPTP = 1.5;}
			else if(this.NPType == "Arts"){NPTP = 1.0;}
			else if(this.NPType == "Quick"){NPTP = 0.8;}
			
			var RCTP=1.0;
			if (this.RCType == "被克制"){RCTP = 0.5;}
			else if(this.RCType == "无克制"){RCTP = 1.0;}
			else if(this.RCType == "通常克制"){RCTP = 2.0;}
			else if(this.RCType == "狂阶克制"){RCTP = 1.5;}
			
			var HDTP=1.0;
			if (this.HDType == "被克制"){HDTP = 0.9;}
			else if(this.HDType == "无克制"){HDTP = 1.0;}
			else if(this.HDType == "克制"){HDTP = 1.1;}
			
			var CL=1.0;
			if (this.Class == "Caster"){CL = 0.9;}
			else if(this.Class == "Assassin"){CL = 0.9;}
			else if(this.Class == "Archer"){CL = 0.95;}
			else if(this.Class == "Saber"){CL = 1.0;}
			else if(this.Class == "Alterego"){CL = 1.0;}
			else if(this.Class == "MoonCancer"){CL = 1.0;}
			else if(this.Class == "Foreigner"){CL = 1.0;}
			else if(this.Class == "Shielder"){CL = 1.0;}
			else if(this.Class == "Lancer"){CL = 1.05;}
			else if(this.Class == "Berserker"){CL = 1.1;}
			else if(this.Class == "Ruler"){CL = 1.1;}
			else if(this.Class == "Avenger"){CL = 1.1;}
			
			var UN = ((this.form.ATK1 + this.form.ATK2) * 0.23 * this.form.MUT * 0.01 * NPTP * (1 + this.form.BUFFCARD * 0.01
			 - this.form.BUFFUNCARD * 0.01) * CL * RCTP * HDTP * (1 + this.form.BUFFATK * 0.01
			 - this.form.BUFFDEF * 0.01 - this.form.BUFFSDEF * 0.01) * (1 + this.form.BUFFSATK * 0.01
			 + this.form.BUFFNP * 0.01) * this.form.BUFFSNP * 0.01 * 0.9) + this.form.EXTRA - this.form.UNEXTRA;
			var UP = ((this.form.ATK1 + this.form.ATK2) * 0.23 * this.form.MUT * 0.01 * NPTP * (1 + this.form.BUFFCARD * 0.01
			 - this.form.BUFFUNCARD * 0.01) * CL * RCTP * HDTP * (1 + this.form.BUFFATK * 0.01
			 - this.form.BUFFDEF * 0.01 - this.form.BUFFSDEF * 0.01) * (1 + this.form.BUFFSATK * 0.01
			 + this.form.BUFFNP * 0.01) * this.form.BUFFSNP * 0.01 * 1.1) + this.form.EXTRA - this.form.UNEXTRA;
			this.jieguo1 = "最低伤害为:" + UN.toFixed(0)
			this.jieguo2 = "最高伤害为:" + UP.toFixed(0)
			this.show = true
		}
	}
};
</script>
<style lang="scss" scoped>
	.slot-content {
		text-align:center;
	}
</style>