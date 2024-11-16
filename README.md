mkdir base-token
cd base-token
npm init -y
npm install --save-dev hardhat
npm install @openzeppelin/contracts
npm install --save-dev @nomiclabs/hardhat-ethers ethers
npm install --save-dev chai
require("@nomiclabs/hardhat-ethers");

module.exports = {
  solidity: "0.8.0",
  networks: {
    hardhat: {
      chainId: 1337
    },
    base: {
      url: "https://base-mainnet.infura.io/v3/YOUR_INFURA_PROJECT_ID",
      accounts: [`0x${YOUR_PRIVATE_KEY}`]
    }
  }
};
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";

contract MyToken is ERC20 {
    constructor() ERC20("MyToken", "MTK") {
        _mint(msg.sender, 1000000 * 10 ** decimals());
    }
}
async function main() {
  const [deployer] = await ethers.getSigners();

  console.log("Deploying contracts with the account:", deployer.address);

  const MyToken = await ethers.getContractFactory("MyToken");
  const token = await MyToken.deploy();

  console.log("Token deployed to:", token.address);
}

main()
  .then(() => process.exit(0))
  .catch((error) => {
    console.error(error);
    process.exit(1);
  });
const { expect } = require("chai");

describe("MyToken", function () {
  it("Deployment should assign the total supply of tokens to the owner", async function () {
    const [owner] = await ethers.getSigners();

    const MyToken = await ethers.getContractFactory("MyToken");

    const hardhatToken = await MyToken.deploy();

    const ownerBalance = await hardhatToken.balanceOf(owner.address);
    expect(await hardhatToken.totalSupply()).to.equal(ownerBalance);
  });
});
npx hardhat compile
npx hardhat run scripts/deploy.js --network base
npx hardhat test
