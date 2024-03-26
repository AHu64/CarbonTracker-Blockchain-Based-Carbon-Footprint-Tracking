# CarbonTracker-Blockchain-Based-Carbon-Footprint-Tracking
CarbonTracker uses Web3 technology to accurately track and report companies' carbon footprints, promoting transparency and accountability in the fight against climate change.
import hashlib
import time
from web3 import Web3

class Block:
    def __init__(self, index, timestamp, data, previous_hash):
        self.index = index
        self.timestamp = timestamp
        self.data = data
        self.previous_hash = previous_hash
        self.hash = self.hash_block()

    def hash_block(self):
        sha = hashlib.sha256()
        sha.update(str(self.index).encode('utf-8') +
                   str(self.timestamp).encode('utf-8') +
                   str(self.data).encode('utf-8') +
                   str(self.previous_hash).encode('utf-8'))
        return sha.hexdigest()

class Blockchain:
    def __init__(self):
        self.chain = [self.create_genesis_block()]
    
    def create_genesis_block(self):
        return Block(0, time.time(), "Genesis Block", "0")
    
    def add_block(self, data):
        last_block = self.chain[-1]
        new_block = Block(len(self.chain), time.time(), data, last_block.hash)
        self.chain.append(new_block)
    
    def get_chain(self):
        return [(block.index, block.timestamp, block.data, block.hash) for block in self.chain]

# Simulating Web3 connection (This would normally connect to an Ethereum network)
web3 = Web3(Web3.EthereumTesterProvider())

# Initializing a Blockchain
carbon_tracker = Blockchain()

# Function to add a carbon footprint entry
def add_carbon_footprint(company_name, carbon_emission):
    data = f"Company: {company_name}, Carbon Emission: {carbon_emission} tonnes"
    carbon_tracker.add_block(data)
    print(f"Added carbon footprint for {company_name}: {carbon_emission} tonnes")

# Function to get the blockchain data
def get_carbon_footprint_data():
    chain_data = carbon_tracker.get_chain()
    for block in chain_data:
        print(f"Block {block[0]}: {block[2]}, HASH: {block[3]}")

# Example Usage
add_carbon_footprint("Company A", 100)
add_carbon_footprint("Company B", 150)
print("\nBlockchain Data:")
get_carbon_footprint_data()
