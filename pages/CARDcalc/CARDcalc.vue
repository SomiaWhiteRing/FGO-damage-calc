<template>
	<view class="wrap">
		<u-form :model="form" ref="uForm" @submit="formSubmit">
			<u-form-item label-width=auto label="英灵攻击力" prop="ATK1"><u-input placeholder="英灵的面板数据" type="number" v-model.number="form.ATK1" /></u-form-item>
			<u-form-item label-width=auto label="礼装攻击力" prop="ATK2"><u-input placeholder="礼装的面板数据" type="number" v-model.number="form.ATK2" /></u-form-item>
			<u-form-item label-width=auto label="首位指令卡">
				<u-radio-group v-model="FirstCARDType">
					<u-radio v-for="(item, index) in FirstCARDTypelist" :key="index" :name="item.name" :disabled="item.disabled">
						{{ item.name }}
					</u-radio>
				</u-radio-group>
			</u-form-item>
			<u-form-item label-width=auto label="指令卡类型">
				<u-radio-group v-model="CARDType">
					<u-radio v-for="(item, index) in CARDTypelist" :key="index" :name="item.name" :disabled="item.disabled">
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
			<u-form-item label-width=auto label="指令卡位置">
				<u-radio-group v-model="CARDPeace">
					<u-radio v-for="(item, index) in CARDPeacelist" :key="index" :name="item.name" :disabled="item.disabled">
						{{ item.name }}
					</u-radio>
				</u-radio-group>
			</u-form-item>
			<u-form-item label-width=auto label="攻击力提升(%)" prop="BUFFATK"><u-input placeholder="从者获得的攻击力类BUFF" type="number" v-model.number="form.BUFFATK" /></u-form-item>
			<u-form-item label-width=auto label="防御力提升(%)" prop="BUFFDEF"><u-input placeholder="敌方持有的防御力类BUFF" type="number" v-model.number="form.BUFFDEF" /></u-form-item>
			<u-form-item label-width=auto label="暴击威力(%)" prop="BUFFPOWER"><u-input placeholder="从者获得的暴击威力类BUFF" type="number" v-model.number="form.BUFFPOWER" /></u-form-item>
			<u-form-item label-width=auto label="指令卡性能提升(%)" prop="BUFFCARD"><u-input placeholder="从者获得的色卡威力提升BUFF" type="number" v-model.number="form.BUFFCARD" /></u-form-item>
			<u-form-item label-width=auto label="指令卡耐性(%)" prop="BUFFUNCARD"><u-input placeholder="敌方持有的色卡威力下降BUFF" type="number" v-model.number="form.BUFFUNCARD" /></u-form-item>
			<u-form-item label-width=auto label="特攻效果(%)" prop="BUFFSATK"><u-input placeholder="从者获得的特攻类BUFF" type="number" v-model.number="form.BUFFSATK" /></u-form-item>
			<u-form-item label-width=auto label="特防效果(%)" prop="BUFFSDEF"><u-input placeholder="敌方持有的特防类BUFF" type="number" v-model.number="form.BUFFSDEF" /></u-form-item>
			<u-form-item label-width=auto label="伤害附加" prop="BUFFEXTRA"><u-input placeholder="从者获得的伤害附加类BUFF" type="number" v-model.number="form.EXTRA" /></u-form-item>
			<u-form-item label-width=auto label="伤害减免" prop="BUFFUNEXTRA"><u-input placeholder="敌方持有的伤害减免类BUFF" type="number" v-model.number="form.UNEXTRA" /></u-form-item>
			<u-form-item label-width=auto label="是否形成BraveChain">
				<u-radio-group v-model="BraveChain">
					<u-radio v-for="(item, index) in BraveChainlist" :key="index" :name="item.name" :disabled="item.disabled">
						{{ item.name }}
					</u-radio>
				</u-radio-group>
			</u-form-item>
			<u-form-item label-width=auto label="是否形成BusterChain">
				<u-radio-group v-model="BusterChain">
					<u-radio v-for="(item, index) in BusterChainlist" :key="index" :name="item.name" :disabled="item.disabled">
						{{ item.name }}
					</u-radio>
				</u-radio-group>
			</u-form-item>
			<u-form-item label-width=auto label="是否暴击">
				<u-radio-group v-model="POWERType">
					<u-radio v-for="(item, index) in POWERTypelist" :key="index" :name="item.name" :disabled="item.disabled">
						{{ item.name }}
					</u-radio>
				</u-radio-group>
			</u-form-item>
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
				BUFFATK:'',
				BUFFDEF:'',
				BUFFCARD:'',
				BUFFUNCARD:'',
				BUFFSATK:'',
				BUFFSDEF:'',
				BUFFPOWER:'',
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
			CARDTypelist: [
				{name: 'Buster'},
				{name: 'Art'},
				{name: 'Quick'},
				{name: 'Extra'}
			],
			CARDType: 'Buster',
			FirstCARDTypelist: [
				{name: 'Buster'},
				{name: 'Art'},
				{name: 'Quick'}
			],
			FirstCARDType: 'Buster',
			CARDPeacelist: [
				{name: '1st'},
				{name: '2nd'},
				{name: '3rd'}
			],
			CARDPeace: '1st',
			POWERTypelist:[
				{name: 'Yes'},
				{name: 'No'}
			],
			POWERType:"No",
			BusterChainlist:[
				{name: 'Yes'},
				{name: 'No'}
			],
			BusterChain:"No",
			BraveChainlist:[
				{name: 'Yes'},
				{name: 'No'}
			],
			BraveChain:"No",
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
			switchVal: false,
			content:'已将数据设置为：<br>我方：阿尔托莉雅·潘德拉贡（saber）<br>敌方：骷髅兵（saber）<br>时的情况',
		};
	},
	
	methods:{
		setdefault(){
			this.form.ATK1=12221
			this.form.ATK2=0
			this.form.BUFFATK=0
			this.form.BUFFDEF=0
			this.form.BUFFCARD=0
			this.form.BUFFUNCARD=0
			this.form.BUFFSATK=0
			this.form.BUFFSDEF=0
			this.form.BUFFPOWER=0
			this.form.EXTRA=0
			this.form.UNEXTRA=0
			this.Class="Saber"
			this.FirstCARDType="Buster"
			this.CARDType="Buster"
			this.CARDPeace="1st"
			this.RCType="无克制"
			this.HDType="克制"
			this.POWERType="No"
			this.BraveChain="No"
			this.BusterChain="No"
			this.set=true
		},
		submit(){
			
			var FCDTP=0;
			if (this.FirstCARDType == "Buster"){FCDTP = 0.5;}
			else if(this.FirstCARDType == "Arts"){FCDTP = 0;}
			else if(this.FirstCARDType == "Quick"){FCDTP = 0;}
			
			var CDTP=1.0;
			if (this.CARDType == "Buster"){CDTP = 1.5;}
			else if(this.CARDType == "Arts"){CDTP = 1.0;}
			else if(this.CARDType == "Quick"){CDTP = 0.8;}
			else if(this.CARDType == "Extra"){CDTP = 1.0;}
			
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
			
			var RCTP=1.0;
			if (this.RCType == "被克制"){RCTP = 0.5;}
			else if(this.RCType == "无克制"){RCTP = 1.0;}
			else if(this.RCType == "通常克制"){RCTP = 2.0;}
			else if(this.RCType == "狂阶克制"){RCTP = 1.5;}
			
			var HDTP=1.0;
			if (this.HDType == "被克制"){HDTP = 0.9;}
			else if(this.HDType == "无克制"){HDTP = 1.0;}
			else if(this.HDType == "克制"){HDTP = 1.1;}
			
			var CDPS=1.0;
			if (this.CARDPeace == "1st"){CDPS = 1.0;}
			else if(this.CARDPeace == "2nd"){CDPS = 1.2;}
			else if(this.CARDPeace == "3rd"){CDPS = 1.4;}
			if(this.CARDType == "Extra"){CDPS = 1.0;}
			
			var EXTRAPLUS=2.0;
			if (this.BraveChain == "Yes"){EXTRAPLUS = 3.5;}
			if (this.CARDType == "Buster"){EXTRAPLUS = 1;}
			if (this.CARDType == "Arts"){EXTRAPLUS = 1;}
			if (this.CARDType == "Quick"){EXTRAPLUS = 1;}
			
			var BUSTERPLUS=0;
			if (this.BusterChain == "Yes"){BUSTERPLUS = (this.form.ATK1 + this.form.ATK2)*0.2;}
			if (this.CARDType == "Extra"){BUSTERPLUS = 0;}
			
			var POWER=0;
			if (this.POWERType == "Yes"){POWER = 1;}
			if (this.CARDType == "Extra"){POWER = 0;}
			
			var UN = ((this.form.ATK1 + this.form.ATK2) * 0.23 * ((CDTP * CDPS * (1 + this.form.BUFFCARD * 0.01
			 - this.form.BUFFUNCARD * 0.01)) + FCDTP) * CL * RCTP * HDTP * (1 + this.form.BUFFATK * 0.01
			 - this.form.BUFFDEF * 0.01 - this.form.BUFFSDEF * 0.01) * (1 + POWER) * (1 + this.form.BUFFSATK * 0.01
			 + this.form.BUFFPOWER * 0.01 * POWER) * EXTRAPLUS * 0.9) + this.form.EXTRA - this.form.UNEXTRA + BUSTERPLUS;
			var UP = ((this.form.ATK1 + this.form.ATK2) * 0.23 * ((CDTP * CDPS * (1 + this.form.BUFFCARD * 0.01
			 - this.form.BUFFUNCARD * 0.01)) + FCDTP) * CL * RCTP * HDTP * (1 + this.form.BUFFATK * 0.01
			 - this.form.BUFFDEF * 0.01 - this.form.BUFFSDEF * 0.01) * (1 + POWER) * (1 + this.form.BUFFSATK * 0.01
			 + this.form.BUFFPOWER * 0.01 * POWER) * EXTRAPLUS * 1.1) + this.form.EXTRA - this.form.UNEXTRA + BUSTERPLUS;
			this.jieguo1 = "最低伤害为:" + UN.toFixed(0)
			this.jieguo2 = "最高伤害为:" + UP.toFixed(0)
			this.show = true
		},
	},
};

</script>

<style lang="scss" scoped>
	.slot-content {
		text-align:center;
	}
</style>