---
title: 클레이튼 수업 2
subtitle: 마켓 앱
date: 2022-02-07
slug: 마켓 앱 컨트렉트 제작
author: han
rating: 5
coverImage: 
---

# NFTsimple.sol source

pragma solidity >=0.4.24 <0.5.6; 

contract IKIP17Receiver {
    function onKIP17Received(
        address operator,
        address from,
        uint256 tokenId,
        bytes memory data
    ) public returns (bytes4);
}

contract NFTsimple {
    string public name = "Han";
    string public symbol = "K-han";

    mapping (uint256 => address) public tokenOwner;
    mapping (uint256 => string) public tokenURIs;

    mapping (address => uint256[]) private _ownedTokens;
    // transferFrom(from, to, tokenId)
    bytes4 private constant _KIP17_RECEIVED = 0x6745782b;



    function mintWithTokenURI(address to, uint256 tokenId, string memory tokenURI) public returns (bool)
    {
        //to에게 tokenId(일련번호)를 발행하겠다.
        //적힌 글자는 tokenURI
        tokenOwner[tokenId] = to;
        tokenURIs[tokenId] = tokenURI;
        //add token to the list
        _ownedTokens[to].push(tokenId);
    }

    function safeTransferFrom(address from, address to, uint256 tokenId, bytes memory _data) public {
        require(from == msg.sender, "from != msg.sender");
        require(from == tokenOwner[tokenId], "you are not the owner of the token");

        _removeTokenFromList(from, tokenId);
        _ownedTokens[to].push(tokenId);
        tokenOwner[tokenId] = to;

        //만약에 받는 쪽이 실행할 코드가 있는 스마트 컨트랙트이면 코드를 실행할 것
        require(
            _checkOnKIP17Received(from, to, tokenId, _data), "KIP17: transfer to non KIP17Receiver implementer"
        );
    }
    
    function _checkOnKIP17Received(address from, address to, uint256 tokenId, bytes memory _data) internal returns (bool) {
        bool success;
        bytes memory returndata;

        if(!isContract(to)) {
            return true;
        }

        (success, returndata) = to.call(
            abi.encodeWithSelector(
                _KIP17_RECEIVED,
                msg.sender,
                from,
                tokenId,
                _data
            )
        );
        if (
            returndata.length !=0 &&
            abi.decode(returndata, (bytes4)) == _KIP17_RECEIVED
        ) {
            return true;
        }
        return false;
    }
    function isContract(address account) internal view returns (bool) {
        uint256 size;
        assembly { size := extcodesize(account)}
        return size > 0;
    }

    function _removeTokenFromList(address from, uint256 tokenId) private {

        uint256 lastTokenIdx = _ownedTokens[from].length - 1;
        for(uint256 i=0; i<_ownedTokens[from].length;i++){
            if(tokenId == _ownedTokens[from][i]){
                 _ownedTokens[from][i] = _ownedTokens[from][lastTokenIdx];
                _ownedTokens[from][lastTokenIdx] = tokenId;
                break;
            }
        }

        _ownedTokens[from].length--;
    }

    function ownedTokens(address owner) public view returns (uint256[] memory) {
        return _ownedTokens[owner];
    }

    function setTokenUri(uint256 id, string memory uri) public {
        tokenURIs[id] = uri;
    }
}

contract NFTMarket {
    mapping(uint256 => address) public seller;
    function buyNFT(uint256 tokenId, address NFTAddress) public payable returns (bool) {
        // 구매한 사람한테 0.01 KLAY 전송
        address payable receiver = address(uint160(seller[tokenId]));

        //send 0.01 KLAY to receiver
        //10 ** 18 PEB = 1 KLAY
        //10 ** 16 PEB = 0.01 KLAY
        receiver.transfer(10 ** 16);
        NFTsimple(NFTAddress).safeTransferFrom(address(this), msg.sender, tokenId, '0x00');

        return true;
    }
// Market이 토큰을 받았을 때(판매대에 올라갔을 때), 판매자가 누구인지 기록해야됨
    function onKIP17Received(address operator, address from, uint256 tokenId, bytes memory data) public returns (bytes4){
        seller[tokenId] = from;

        return bytes4(keccak256("onKIP17Received(address,address,uint256,bytes)"));
    }
}