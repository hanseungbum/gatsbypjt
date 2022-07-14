---
title: 클레이튼 교육
subtitle: 지갑 주소 및 내용 정리
date: 2022-02-04
slug: klaytn class
author: han
rating: 5
coverImage: 
---
 
# 첫번째 지갑 주소
Address
0xc23fd7fd581555a3d706a516cf5b103f4b2edab7

Private Key
0xf70bf945eb52c1e52cee6329b1701cce9ac12dd8f69df801fadc2cd7f8f8a995

Klaytn Wallet Key
0xf70bf945eb52c1e52cee6329b1701cce9ac12dd8f69df801fadc2cd7f8f8a9950x000xc23fd7fd581555a3d706a516cf5b103f4b2edab7

# 두번째 지갑 주소
Address
0xb0d83484b7e99371ee9f6216f33ee38643424395

Private Key
0x0e5cbb5f17cf17dd6b2b45dae8c14fd6409d13c3cd35f080cef2e7da5af396b2

Klaytn Wallet Key
0x0e5cbb5f17cf17dd6b2b45dae8c14fd6409d13c3cd35f080cef2e7da5af396b20x000xb0d83484b7e99371ee9f6216f33ee38643424395

testnet count contract address
0x6EBf91b9Ddc2Ed0F9f9e5AB2BCbCa7f6586783FC

Mainnet count contract address
0x2858185e273ebC554478C1f2E4d21ff494805059

ACCESS_KEY_ID 
KASK96V7OJU45OCSJNMQVUS0

SECRET_ACCESS_KEY 
grQWVk08S_aslUE5s2QYxBbdk458x9LJjRVfGX5t

## Count.sol source

pragma solidity >=0.4.24 <=0.5.6;

contract Count {
    uint256 public count = 0;

    function getBlockNumber() public view returns (uint256) {
        return block.number;
    }

    function setCount(uint256 _count) public {
        count = _count;
    }
}

## FT vs NFT

FT : 대체 가능한 토큰
100원, 500원 ...
 ex) 화폐 

NFT : 대체 불 가능한 토큰
기능)
 1. 발행(id, 글자, 소유자)
 2. 전송(누가, 누구에게, 무엇을)

## practice.sol source

pragma solidity >=0.4.24 <=0.5.6;

contract Practice {
    uint256 private totalSupply = 10;
    string public name = "kayhan";

    address public owner; // contract deployer
    mapping(uint256 => string) public tokenURIs;
    constructor () public {
        owner = msg.sender;
    }
    function getTotalSupply() public view returns (uint256) {
        return totalSupply+1000;
    }

    function setTotalSupply(uint256 newSupply) public {
        require( owner == msg.sender, 'not owner');
        totalSupply = newSupply;
    }

    function setTokenUri(uint256 id, string memory uri) public {
        tokenURIs[id] = uri;
    }
}

## practice.sol 발행, 전송 기능 추가

pragma solidity >=0.4.24 <=0.5.6;

contract Practice {
    string public name = "kayhan";
    string public symbol = "HAN";

    mapping(uint256 => address) public tokenOwner;
    mapping(uint256 => string) public tokenURIs;

    //소유한 토큰 리스트
        mapping(address => uint256[]) private _ownedTokens;

    //발행, 전송 기능 추가

    function minWithTokenURI( address to, uint256 tokenId, string memory tokenURI) public returns (bool) {
        tokenOwner[tokenId] = to;
        tokenURIs[tokenId] = tokenURI;

        _ownedTokens[to].push(tokenId);
        return true;
    }

    function _removeTokenFromList(address from, uint256 tokenId) private {
        uint256 lastTokenIdx = _ownedTokens[from].length - 1;
        for(uint256 i=0; i< _ownedTokens[from].length; i++){
            if(tokenId ==_ownedTokens[from][i]){
                _ownedTokens[from][i] = _ownedTokens[from][lastTokenIdx];
            }
        }
        _ownedTokens[from].length--;
    }
    function safeTransferFrom(address from, address to, uint256 tokenId) public {
        require(from == msg.sender, "from != msg.sender");
        require(from == tokenOwner[tokenId], "u r not the owner");
        _removeTokenFromList(from, tokenId);
        _ownedTokens[to].push(tokenId);
        tokenOwner[tokenId] = to;
    }
    function ownedTokens(address owner) public view returns(uint256[] memory) {
        return _ownedTokens[owner];
    }
    function setTokenUri(uint256 id, string memory uri) public {
        tokenURIs[id] = uri;
    }
}



## contract 연결

NFT 스마트 컨트렉트, Market 스마트 컨트렉트

인터페이스와 각 주소를 알아야함
nft 마다 스마트 컨트렉트 주소

## NFTSimple.sol

pragma solidity >=0.4.24 <=0.5.6;

contract NFTSimple {
    string public name = "kayhan";
    string public symbol = "HAN";

    mapping(uint256 => address) public tokenOwner;
    mapping(uint256 => string) public tokenURIs;

    //소유한 토큰 리스트
        mapping(address => uint256[]) private _ownedTokens;

    //발행, 전송 기능 추가

    function minWithTokenURI( address to, uint256 tokenId, string memory tokenURI) public returns (bool) {
        tokenOwner[tokenId] = to;
        tokenURIs[tokenId] = tokenURI;

        _ownedTokens[to].push(tokenId);
        return true;
    }

    function _removeTokenFromList(address from, uint256 tokenId) private {
        uint256 lastTokenIdx = _ownedTokens[from].length - 1;
        for(uint256 i=0; i< _ownedTokens[from].length; i++){
            if(tokenId ==_ownedTokens[from][i]){
                _ownedTokens[from][i] = _ownedTokens[from][lastTokenIdx];
            }
        }
        _ownedTokens[from].length--;
    }
    function safeTransferFrom(address from, address to, uint256 tokenId) public {
        require(from == msg.sender, "from != msg.sender");
        require(from == tokenOwner[tokenId], "u r not the owner");
        _removeTokenFromList(from, tokenId);
        _ownedTokens[to].push(tokenId);
        tokenOwner[tokenId] = to;
    }
    function ownedTokens(address owner) public view returns(uint256[] memory) {
        return _ownedTokens[owner];
    }
    function setTokenUri(uint256 id, string memory uri) public {
        tokenURIs[id] = uri;
    }
}

contract NFTMarket{
    function buyNFT(uint256 tokenId, address NFTAddress, address to) public returns (bool) {
        NFTSimple(NFTAddress).safeTransferFrom(address(this), to, tokenId);//컨트렉트를 소유하고 있는 주소에 주소가 갖고있는 함수를 요청해야함
        return true;    
    }
}
