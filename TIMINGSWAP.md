Abstract

This technical white paper explains some of the new design decisions behind the Timingswap contract. The core transaction function of Timingswap comes from the factory and router contract of uniswap v2. During the development of Timingswap, the fee collection proportion of feeto has been adjusted, increasing it from 1 / 6 to 1 / 2. All transaction fees shall be entered into the TimingGuarder contract address (i.e. feeto address) and cannot be changed. Compared with uniswap v2, Timingswap adds the transaction volume statistics function and Timingtoken distribution function of token-eth transaction pairs. Therefore, the following text only describes the functions added by Timingswap compared with uniswap v2. See the uniswap v2 white paper for other similar functions. 

![image](https://github.com/Timingswap/whitepaper/blob/main/TIMINGSWAP.jpg)

Introduction

The added functions of Timingswap compared with uniswap v2 are described as follows according to the Contract Name:

1、	Factory:only the statistics function of LP tokens in different pairs is added.The function of restricting the feeto address is added through the timelocker variable. After 100 blocks of the feeto address are set, the feeto address cannot be changed by the feetoseter..

2、	RouterV2:add the transaction volume statistics function getnumber for token-eth transaction pairs (the transaction volume is calculated based on the main network currency ETHER), and add the minting distribution function mintto of Timingtoken (ensure that the exchange will not cast Timingtoken repeatedly by comparing followblock and lastblock, so this function is defined as public and can be called by everyone), The distribution of Timingtoken is divided into two categories, which are different for traders and liquidity providers:

①	The method for traders is to multiply the proportion of each trader's statistical trading volume in the total statistical trading volume in each block by 47.5% of the total minting volume of the Timingtoken in each block.

②	The method for liquidity providers: first, according to the proportion of the statistical trading volume of a pool in each block to the total statistical trading volume in the block, multiply by the proportion of LP token supply volume of liquidity providers in the pool, and finally multiply by 47.5% of the total minting volume of Timingtoken in each block. (999827 / 1000000 is the adjustment factor for the increase of minting quantity due to the influence of initial liquidity of pool locking).

③	5% of the total minting quantity of Timingtoken in each block is mint to the developer's address.
Total minting capacity of Timingtoken: it is designed according to the principle of halving every 180 days. The theoretical total supply is no more than 21 million. The output of each block varies with the time of block out in different main networks (for example, the ETH main network has 4.41 blocks per minute, so the initial output of each block is designed to be 9.186765070277834; the BSC main network has 20 blocks per minute, so the initial output of each block is designed to be 2.025462962962; the heco main network has 20 blocks per minute, so the initial output of each block is designed to be 2.025462962962) , the corresponding Timingtoken of all Timingswap blocks without transaction volume cannot be digged out (equivalent to be burned).

3、	TimingGuarder:The feeto service charge of Timingswap is increased from 1 / 6 of uniswap v2 to 1 / 2, and the TimingGuarder contract address is setted to the feeto address. The contract has two main functions: (function convertworth and function getback)

① function convertweth is used to convert all the service charges (the service charges are entered in the form of LP token) into weth. This function needs to be manually called by the user to obtain the latest baseprice (calculate the base price according to the weth stock in the contract corresponding to the total supply of Timingtoken at this time);

② the function getback is to automatically return the corresponding number of weth to the address of Timingtoken senders according to the base price in the contract and burn the Timingtoken in the contract.

4、	TimingToken: The ownership of Timingtoken has been transferred to the router contract, The token contract is based on the sushi token contract, only minor modifications are made, and the function burn method, function Tapprove authorization method and function transferfrom method are added

Disclaimers

This document is for reference only. It does not constitute an investment proposal or an offer or solicitation to buy or sell any investment, nor should it be used to assess the merits of making any investment decision. It should not be used for accounting, legal or tax advice or investment advice. This article only reflects the author's current views. The opinions reflected here may change and will not be updated.

![image](https://github.com/Timingswap/whitepaper/blob/main/TIMINGSWAP%20F.png)
