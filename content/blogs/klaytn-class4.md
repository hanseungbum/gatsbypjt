---
title: 클레이튼 수업 4
subtitle: caver.js
date: 2022-02-07
slug: 리액트 앱 개발 환경 설정
author: han
rating: 5
coverImage: 
---

# klayten doc
uri : https://ko.docs.klaytn.com/
참고

# 오늘의 할일

1 smart contract 배포 주소 가져오기
2 caver.js 이용해서 스마트 컨트렉트 연동
3 가져온 스마트 컨트렉트 실행 결과 웹에 표현

# ABI(Application Binary InterFace)
인터페이스


## App.js Count.sol 코드와 연동하기

import logo from './logo.svg';
import Caver from 'caver-js';
import './App.css';

const COUNT_CONTRACT_ADDRESS = '0xf4De4bB9618793E6aDdD19bBD5640a2326178E8c';
const ACCESS_KEY_ID = 'KASK96V7OJU45OCSJNMQVUS0';
const SECRET_ACCESS_KEY = 'grQWVk08S_aslUE5s2QYxBbdk458x9LJjRVfGX5t';
const CHAIN_ID = '1001';
const COUNT_ABI = '[ { "constant": true, "inputs": [], "name": "count", "outputs": [ { "name": "", "type": "uint256" } ], "payable": false, "stateMutability": "view", "type": "function" }, { "constant": true, "inputs": [], "name": "getBlockNumber", "outputs": [ { "name": "", "type": "uint256" } ], "payable": false, "stateMutability": "view", "type": "function" }, { "constant": false, "inputs": [ { "name": "_count", "type": "uint256" } ], "name": "setCount", "outputs": [], "payable": false, "stateMutability": "nonpayable", "type": "function" } ]';
const option = {
  headers : [
    {
      name: "Authorization",
      value: "Basic "+ Buffer.from(ACCESS_KEY_ID + ":" + SECRET_ACCESS_KEY).toString("base64")
    },
    {name: "x-chain-id", value : CHAIN_ID}
  ]
}
const caver = new Caver(new Caver.providers.HttpProvider("https://node-api.klaytnapi.com/v1/klaytn",option));
const CountContract = new caver.contract(JSON.parse(COUNT_ABI), COUNT_CONTRACT_ADDRESS);
const readCount = async () => {
  const _count = await CountContract.methods.count().call();
  console.log(_count);
}

const getBalance = (address) => {
  return caver.rpc.klay.getBalance(address).then((res)=>{
    const balance = caver.utils.convertFromPeb(caver.utils.hexToNumberString(res));
    console.log(`BALANCE: ${balance}`);
    return balance;
  })
}

const setCount = async (newCount) => {
  try{
    const privatekey = '0xf70bf945eb52c1e52cee6329b1701cce9ac12dd8f69df801fadc2cd7f8f8a995';
    const deployer = caver.wallet.keyring.createFromPrivateKey(privatekey);
    caver.wallet.add(deployer);
    const receipt = await CountContract.methods.setCount(newCount).send({
      from: deployer.address,//address,
      gas: "0x4bfd200"//
    })
    console.log(receipt);
  }catch(e){
    console.log(`[ERROR_SET_COUNT]${e}`);
  }
 
} 
function App() {
  readCount();
  getBalance('0xc23Fd7Fd581555a3D706a516Cf5B103f4b2eDaB7');
  return (
    <div className="App">
      <header className="App-header">
        <img src={logo} className="App-logo" alt="logo" />
        <button title={'카운트 변경'} onClick={()=>{setCount(100)}}/>
        <p>
          Edit <code>src/App.js</code> and save to reload.
        </p>
        <a
          className="App-link"
          href="https://reactjs.org"
          target="_blank"
          rel="noopener noreferrer"
        >
          Learn React
        </a>
      </header>
    </div>
  );
}

export default App;
