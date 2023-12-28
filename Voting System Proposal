// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract DAO {

		// Struct to represent a proposal in the DAO	
   struct Proposal {
	  string description;
    uint voteCount;
    uint yesVotes;
    uint noVotes;
    bool executed;
 }
		
		// Struct to represent a member of the DAO
		struct Member {
		    address memberAddress;
		    uint memberSince;
		    uint tokenBalance;
		} 
		
		address[] public members;
    mapping(address => bool) public isMember;
    mapping(address => Member) public memberInfo;
    mapping(address => mapping(uint => bool)) public votes; 
    Proposal[] public proposals;
    address public owner;
		
		uint public totalSupply; // Total token supply
		mapping(address => uint) public balances; // Mapping to store token balances of members
		
		event ProposalCreated(uint indexed proposalId, string description); // Event emitted when a proposal is created
		event VoteCast(address indexed voter, uint indexed proposalId, uint tokenAmount); // Event emitted when a vote is cast
		event ProposalAccepted(string message); // Event emitted when a proposal is accepted
		event ProposalRejected(string rejected); // Event emitted when a proposal is rejected
		
    constructor(){
        owner = msg.sender;
    }

		// Function to add a member to the DAO
		function addMember(address _member) public {
      require(msg.sender == owner);
      require(isMember[_member] == false, "Member already exists");
      memberInfo[_member] = Member({
          memberAddress: _member,
          memberSince: block.timestamp,
          tokenBalance: 100
      });
      members.push(_member);
      isMember[_member] = true;
      balances[_member] = 100;
      totalSupply += 100;
   }
		
		// Function to remove a member from the DAO
		function removeMember(address _member) public {
    require(isMember[_member] == true, "member does not exist");
    memberInfo[_member] = Member({
        memberAddress: address(0),
        memberSince: 0,
        tokenBalance: 0
    });
    for (uint i = 0; i < members.length; i++) {
        if (members[i] == _member) {
            members[i] = members[members.length - 1];
            members.pop();
            break;
        }
    }
    isMember[_member] = false;
    balances[_member] = 0;
    totalSupply -= 100;
  }
		
		// Function to create a proposal in the DAO
       function createProposal(string memory _description) public {
		    proposals.push(Proposal({
		        description: _description,
		        voteCount: 0,
                yesVotes: 0,
                noVotes: 0,
		        executed: false
		    }));
		    emit ProposalCreated(proposals.length - 1, _description);
		}
		
		// Function for a member to vote on a proposal
		function voteYes(uint _proposalId, uint _tokenAmount) public {
    require(isMember[msg.sender] == true, "You should be a memeber to vote");
    require(balances[msg.sender] >= _tokenAmount, "Not enough tokens to vote");
    require(votes[msg.sender][_proposalId] == false, "You have already voted for this proposal");
    votes[msg.sender][_proposalId] = true;
    memberInfo[msg.sender].tokenBalance -= _tokenAmount;
    proposals[_proposalId].voteCount += _tokenAmount;
    proposals[_proposalId].yesVotes += _tokenAmount;
    emit VoteCast(msg.sender, _proposalId, _tokenAmount);
  }		
  
  function voteNo(uint _proposalId, uint _tokenAmount) public {
    require(isMember[msg.sender] == true, "You should be a memeber to vote");
    require(balances[msg.sender] >= _tokenAmount, "Not enough tokens to vote");
    require(votes[msg.sender][_proposalId] == false, "You have already voted for this proposal");
    votes[msg.sender][_proposalId] = true;
    memberInfo[msg.sender].tokenBalance -= _tokenAmount;
    proposals[_proposalId].voteCount += _tokenAmount;
    proposals[_proposalId].noVotes += _tokenAmount;
    emit VoteCast(msg.sender, _proposalId, _tokenAmount);
  }
  
		// Function to execute a proposal in the DAO
	function executeProposal(uint _proposalId) public {
      require(proposals[_proposalId].executed == false, "Proposal has already been executed");
      require(proposals[_proposalId].yesVotes > proposals[_proposalId].noVotes, "Do not have enough votes");
      proposals[_proposalId].executed = true;
      emit ProposalAccepted("Proposal has been approved");
}
}
