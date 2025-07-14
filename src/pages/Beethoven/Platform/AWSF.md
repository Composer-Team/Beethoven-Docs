# AWS F2 Implementation Flow

Our AWS flow uses the HDK from the AWS FPGA Github: [link](https://www.github.com/aws/aws-fpga).

> Note: The previous F1 flow provided a high-bandwidth DMA driver that used XDMA. The release of the F2 instances
> did not include a DMA driver. While the AWS folks [work on adding this functionality back](https://github.com/aws/aws-fpga/issues/671),
> we provide a workaround that provides low-bandwidth data transfer, but should function for experimentation in the
> time being.

Prior to implementing your accelerator on the F2 instance, do debug in simulation. After your design
simulates successfully, you'll have something that, if it passes timing, will work on the FPGA.

## Security Credentials

Setup an AWS account and, if applicable, sign into your organization so that you don't personally get billed for all of this.
In your EC2 dashboard on the left sidebar, click **Key Pairs**. Under **Actions** at the top right, import a key pair from your
computer. If you do not have an existing keypair, you can generate one using

```sh
# Generate a keypair and store it in ~/.ssh/id_ed25519[.pub]
ssh-keygen -t ed25519
```

You should import the public key (e.g., `~/.ssh/id_ed25519.puyb`). Give the key an identifiable name.

Next, we'll generate the AWS keys. At the top right of the EC2 Dashboard, click on your username and click **Security Credentials**.
Under access keys, generate an access key and save the CSV for these keys somewhere where you'll remember them.

## AWS Instance Setup

In your EC2 dashboard, click **Launch Instances**.
You'll be setting up a "compile" instance first. This will be the (cheaper) CPU instance for compiling your FPGA image and
transferring it over to the FPGA instance.

> FPGAs are programmed using a "bitstream." This is similar to a normal executable in that it provides the program for the
> FPGA except instead of storing a sequence of instructions, it stores the layout of the circuit. This compilation process
> is long (many hours/days) and compute intensive. These circuit compilers are called EDA (Electronic Design Automation) tools
> and there is a large community that supports their development and [research](https://www.dac.com).

In the Amozon Machine Image (AMI) search box, look up "fpga". In the next window, click **AWS Marketplace AMIs** and select
the **FPGA Developer AMI**. I'm currently using version 1.16.1, but this may change in the future.

When it takes you back to the previous page with the the AMI now selected, choose the instance type. We recommend something
with the best single-core performance and at least 32GB of RAM. We're currently using `m5zn.2xlarge`.

Finally, in the key-pair selection box, select the key-pair you previously uploaded, and launch the instance. The first
time starting an instance may take a while. As well, it may tell you lack the quota for the requested instance. Go through
the process of increasing your quote (may take a day or so).

Once you've launched an instance, it will start consuming your money, so shut it down if you're not actively using it.

## Compiling

Once you have a functioning hardware design and are ready to deploy on the FPGA, start up your compile instance in the EC2 dashboard.
Turn your hardware buildMode to `Synthesis` and run using the `AWSF2Platform`. When it completes running, it will ask for
the IP address for your compile instance. You can find this by clicking on your running instance in the EC2 dashboard (see under
IPv4 address).

Once entered, your design will transfer over to the instance. Log onto your instance and clone the AWS F2 repo. The following
is some initial setup that you should only do once.

```sh
# change to home directory
cd 

# Clone the aws repo
sudo apt-get install -y git git-lfs python3
git clone https://github.com/aws/aws-fpga

# Setup Beethoven
git clone https://github.com/Composer-Team/Beethoven
echo "export PATH=\$HOME/Beethoven/bin:\$PATH" >> ~/.bashrc
source ~/.bashrc

# Add security credentials
# Follow the prompts and use the entries from the CSV we generated earlier
# Your region is likely us-east-1, but you should be able to see it on the
# EC2 dashboard.
aws configure
```

Next, you'll source the hdk setup. You should do this for **every terminal session**.

```sh
cd ~/aws-fpga
source hdk_setup.sh
```

Finally, compile your FPGA image:
```sh
cd ~/cl_beethoven_top/build/scripts/
python3 aws_build_dcp_from_cl.py --cl cl_beethoven_top
```

This should take a while, after which it'll either report success or failure.
On success, you should be able to run `aws-build-mv`.
Try to keep any associated keys simple: all lower-case, only symbols like `-`.

You will be able to track the availability of your image with

```sh
aws ec2 describe-fpga-images --owner self
```

After a while, the availablility will be marked 'available' and you'll be able to run the image from the F2 instance.

## Running

Repeat the process of launching an instance except instead of an `m5zn.2xlarge`, allocate an `f2` type instance.
We'll select the smallest instance for our purposes.

Once you're in the instance, clone the aws repo and instead of sourcing the HDK setup, source the SDK setup. 
